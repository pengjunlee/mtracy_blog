> 原文链接：<http://www.th7.cn/db/nosql/201510/135382.shtml>

# HDFS的文件append功能
早期版本的HDFS不支持任何的文件更新操作，一旦一个文件创建、写完数据、并关闭之后，这个文件就再也不能被改变了。为什么这么设计？是为了与MapReduce完美配合，MapReduce的工作模式是接受一系列输入文件，经过map和reduce处理，直接产生一系列输出文件，而不是在原来的输入文件上做原位更新。为什么这么做？因为直接输出新文件比原位更新一个旧文件高效的多。

在HDFS上，一个文件一直到它的close方法成功执行之后才会存在，才能被其他的client端所看见。如果某个client端在写文件时或者在close文件时失败了，那么这个文件就不会存在，就好像这个文件从来没写过，唯一恢复这个文件的方法，就是从头到尾重新再写一遍。

Hadoop1.x版本一直都不支持文件的append功能，一直到Hadoop 2.x版本，append 功能才被添加到Hadoop Core中，允许向HDFS文件中追加写数据。为此，HDFS Core 也作出了一些重大的改变，以支持这一操作。append功能添加到HDFS经历了一番曲折和一段很长的时间（具体可以参考http://blog.cloudera.com/blog/2009/07/file-appends-in-hdfs/和 https://issues.apache.org/jira/browse/HADOOP-8230）。

# HBase 如何完成数据更新和删除操作
HBase依赖于HDFS来存储数据。HBase作为数据库，必须提供对HBase表中数据的增删改查，而HDFS的文件只支持append操作、不支持删除和更新操作，那么HBase如何依赖HDFS完成更新以及删除操作呢？

## 更新操作
HBase表中的数据当存放到HDFS中时，在HDFS看来，已经可以简单的理解成key-value对，其中key可以理解成是由：rowkey+column family+column qualifier+timestamp+type 组成。HBase 对新增的数据以及要更新的数据（理解成key-value对），都直接先写入MemStore结构中，MemStore是完全的内存结构，且是key有序的。当MemStore达到一定大小后，该MemStore一次性从内存flush到HDFS中（磁盘中），生成一个HFile文件，HFile文件同样是key有序的，并且是持久化的位于HDFS系统中的。通过这种机制，HBase对表的所有的插入和更新都转换成对HDFS的HFile文件的创建。

你可能会迅速的想到，那查询怎么办？

是的，这种方式解决了插入和更新的问题，而查询就变得相对麻烦。而这也正是HBase设计之初的想法：以查询性能的下降来换取更新性能的提升。

事实上查询是按照如下来完成的。

每当MemStore结构flush到HDFS上时，就会产生一个新的HFile文件，随着时间的推移，会产生一连串的HFile文件，这些HFile文件产生的先后顺序非常的重要，可以想象成他们按创建时间排成一个队列，最近产生的在最前面，较早产生的在最后面。当HBase执行查询操作时（可以理解为给出key，要找到value），首先查询内存中的MemStroe结构，如果命中，就返回结果，因为MemStore中的数据永远是最新的，如果不命中，就从前到后遍历之前产生的HFile文件队列，在每个HFile文件中查找key，看是否命中，如果命中即可返回（最新的数据排在最前面），如果不命中一直查找下去，直到所有HFile文件被搜索完结束。

由此可见，查询操作最坏情况下可能要遍历所有HFile文件，最好情况下在内存中MemStore即可命中，这也是为什么HBase查询性能波动大的原因。当然HBase也不会真的很傻的去遍历每个HFile文件中的内容，这个性能是无法忍受的，它采取了一些优化的措施：1、引入bloomfilter，对HFile中的key进行hash，当查询时，对查询key先过bloomfilter，看查询key是否可能在该HFile中，如果可能在，则进入第2步，不在则直接跳过该HFile；2、还记得吗？HFile是key有序的（具体实现是类SSTable结构），在有序的key上查找就有各种优化技术了，而不是单纯的遍历了。

通过以上机制，HBase很好的解决了插入和更新、以及查找的问题，但是问题还没有结束。细心的朋友很可能已经看出来，上述过程中，HFile文件一直在产生，HFile文件组成的列表一直在增大，而计算机资源是有限的，并且查询的性能也依赖HFile队列的长度，因此我们还需要一种合并HFile文件的机制，以保持适度的HFile文件个数。HBase中实现这种机制采用的是LSM树（一种NOSQL系统广泛使用的结构），LSM能够将多个内部key有序的小HFile文件合并生成一个大的HFile文件，当新的大的HFile文件生成后，HBase就能够删除原有的一系列旧的小的HFile文件，从而保持HFile队列不至于过长，查询操作也不至于查询过多的HFile文件。在LSM合并HFile的时候，HBase还会做很重要的两件事：1、将更新过的数据的旧版本的数据删除掉，只留下最新的版本；2、将标有删除标记（下面一节会讲到）的数据删除掉。

## 删除操作
有了以上机制，HBase完成删除操作非常的简单，对将要删除的key-value对进行打标，通常是对key进行打标，将key中的type字段打标成“删除”标记，并将打标后的数据append到MemStore中，MemStore再flush到HFile中，HFile合并时，检查这个标记，所有带有“删除”标记的记录将被删除而不会合并到新的HFile中，这样HBase就完成了数据的删除操作。

# HBase 的WAL
HBase的WAL（Write-Ahead-Log）机制是必须的，一个RegionServer通常与一个HLog一一对应，数据写入Region之前先写HLog能够保障数据的安全。 HLog使用Hadoop的SequenceFile存储日志，而HLog是一直连续不断追加写文件的，它强烈依赖SequenceFile的append功能。事实上正是HLog对append功能的强烈需求，或多或少推动了HDFS在最近的版本中添加了文件追加功能。