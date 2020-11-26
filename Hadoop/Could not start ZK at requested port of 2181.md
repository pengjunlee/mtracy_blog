> 原文地址： <https://www.cnblogs.com/huomei/p/12112800.html>

HMaster和HRegionServer是Hbase的两个子进程，但是使用jps发现没有启动起来，所以去我们配置的logs查看错误信息。提示：

	Could not start ZK at requested port of 2181.  ZK was started at port: 2182.  Aborting as clients (e.g. shell) will not be able to find this ZK quorum.

但是在`hbase-env.sh`文件中设置了`export HBASE_MANAGES_ZK=false`，设置不使用自带Zookeeper，这一步设置完按理说就可以使用独立的Zookeeper程序了,但是还是报错。很明显,这是启动自带Zookeeper与独立Zookeeper冲突了。因为把`hbase.cluster.distributed`设置为`false`,也就是让hbase以standalone模式运行时,依然会去启动自带的zookeeper。

所以要做如下设置,值为**true**：`vim conf/hbase-site.xml`

	<property>
	    <name>hbase.cluster.distributed</name>
	    <value>true</value> 
	</property>
 