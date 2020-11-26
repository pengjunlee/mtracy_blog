> 原文地址： <https://blog.csdn.net/skywalker_only/article/details/39120455>

在**Hadoop-2.x**版本之前只存在`SecondaryNameNode`，没有`CheckpointNode`、`BackupNode`的概念，在**2.x**版本中引入了后两者，增强了对`NameNode`的同步和备份。现在就学习一下2.x版本中的`SecondaryNameNode`、`CheckpointNode`、`BackupNode`，在开始之前先了解一下`NameNode`中的两个重要文件`fsimage`和`edits`以及`NameNode`是如何使用这两个文件持久化命名空间的，比如HDFS文件系统的元数据。

`NameNode`使用`fsimage`和`edits`持久化命名空间，其中`fsimage`保存最新的检查点信息，`edits`保存自最新检查点后的命名空间的变化。当`NameNode`启动时，`NameNode`从`fsimage`中读取HDFS的状态，并会合并`edits`中信息到`fsimage`中以提供最新的文件系统元数据，这样`fsimage`中保存了HDFS的最新状态并会创建新的空的`edits`文件。由于`fsimage`和`edits`的合并只在`NameNode`启动时执行，如果`NameNode`长时间没有重新启动过，`edits`日志文件将会变得非常大，另一个`edits`日志文件太大的副作用是下次`NameNode`的重启将会花费更多的时间，因为需要更多的时间合并`fsimage`和`edits`文件。

由上面的描述可知`fsimage`和`edits`文件对整个HDFS有着至关重要的作用，一旦`NameNode`失效，比如宕机无法启动或者硬盘损坏，就将导致由于失去`fsimage`和`edits`文件而无法启动HDFS的现象，所以就出现了`SecondaryNameNode`、`CheckpointNode`、`BackupNode`用于对fsimage和edits文件备份，`SecondaryNameNode`是最先存在的，后两者是在**2.x**版本引入的。

`SecondaryNameNode`定期性地合并`fsimage`和`edits`文件，并保证`edits`日志文件大小不超过某个阈值。

`SecondaryNameNode`与`NameNode`运行在不同的主机上，并且内存要与`NameNode`的内存一样大小。

`SecondaryNameNode`将包含最新检查点信息的`fsimage`存储在与`NameNode`目录结构一致的目录中，这样`SecondaryNameNode`的`fsimage`总是在必要时可以被`NameNode`读取。`SecondaryNameNode`上检查点进程的启动被下面两个参数控制：

- dfs.namenode.checkpoint.period，指定了两次连续的检查点之间的最大间隔，默认值为3600秒，即1小时
- dfs.namenode.checkpoint.txns，定义了`NameNode`上未检查的事务的数量，当`NameNode`上未检查的事务的数量达到该值时，将会启动紧急检查点而不管 dfs.namenode.checkpoint.period 设置的时间间隔是否有效，默认值为1000000。

`CheckpointNode`定期地创建命名空间的检查点。具体是如何做的呢？首先从`NameNode`下载`fsimage`和`edits`，然后在本地合并它们，然后将合并后的新的`fsimage`上传回`NameNode`，这样就避免了重新启动`NameNode`再合并`fsimage`和`edits`的问题。`CheckpointNode`与`NameNode`通常运行在不同的机器上，内存与`NameNode`的内存一样大小。使用参数`dfs.namenode.backup.address`设置`CheckpointNode`的地址，参数`dfs.namenode.backup.http-address`设置`CheckpointNode`的web地址，使用命令`bin/hdfs namenode –checkpoint`启动`CheckpointNode`。`CheckpointNode`控制检查点进程的参数与`SecondaryNameNode`上的参数一致，也是`dfs.namenode.checkpoint.period`和`dfs.namenode.checkpoint.txns`。`CheckpointNode`上保存`fsimage`和`edits`文件的目录结构与`NameNode`上的一致。

`BackupNode`不仅提供了与`CheckpointNode`相同的检查点功能，而且在内存中维护了文件系统命名空间的最新拷贝，该拷贝与`NameNode`中的状态总是同步的。除了从`NameNode`接收`edits`文件流外，`BackupNode`将这些edits文件在内存中创建副本，这样就创建了命名空间的备份。`BackupNode`不需要从`NameNode`下载`fsimage`和`edits`文件以创建检查点（`CheckpointNode`和`SecondaryNameNode`需要从`NameNode`下载这些文件），因为`BackupNode`已经在内存中最新的命名空间状态。`BackupNode`的检查点更加高效，因为它只需要将命名空间保存到本地`fsimage`文件中并重置`edits`文件。`BackupNode`的内存大小与`NameNode`的一样。

`NameNode`一次只支持一个`BackupNode`，并且在启用`BackupNode`的同时不启用`CheckpointNode`。这一点也可以从参数`dfs.namenode.backup.addres`和`dfs.namenode.backup.http-addres`推断而出，因为配置`BackupNode`和`CheckpointNode`都是这两个参数。使用命令`bin/hdfs namenode –backup`启动`BackupNode`。`BackupNode`为`NameNode`运行而不需要持久化存储提供了选项，可以将持久化命名空间状态的责任委托给`BackupNode`，因为`BackupNode`实时地从`NameNode`取得命名空间的状态。要想实现该目的，在启动`NameNode`时使用`-importCheckpoint`选项，并在配置文件中将参数`dfs.namenode.edits.dir`设置为空。

上面的学习描述了**Hadoop-2.x**版本中`SecondaryNameNode`、`CheckpointNode`和`BackupNode`的工作机制，通过学习可以发现在**Hadoop-2.x**中，对`NameNode`中命名空间状态的备份更加强大和高效，整个集群也因此具有更高的可用性和数据完整性，对于更多的细节，比如轮询、传输文件流到`BackupNode`，则需要结合配置文件中的相关参数和源代码进行更深层次的学习。