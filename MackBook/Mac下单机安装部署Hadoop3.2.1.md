# 环境准备
## 修改主机名

	sudo scutil --set HostName localhost

## ssh免密登录

	ssh-keygen -t rsa      （一路回车直到完成）
	cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
	chmod og-wx ~/.ssh/authorized_keys

设置完之后`ssh localhost`不需要输入密码便登录，就表示设置成功。

## JDK环境
在安装hadoop之前，jdk环境已经配置好了，但如果版本不合适，需要重新下载，在环境变量（`～/.bash_profile`）中修改路径就可以了，不需要卸载以前的jdk。

查看jdk版本：

	$ java -version
	java version "1.8.0_171"
	Java(TM) SE Runtime Environment (build 1.8.0_171-b11)
	Java HotSpot(TM) 64-Bit Server VM (build 25.171-b11, mixed mode)

# 安装Hadoop
## 配置环境变量
将`hadoop-3.2.1.tar.gz`解压至安装目录`/usr/local/hadoop-3.2.1`。

接下来，在`~/.bash_profile`中增加Hadoop相关用户环境变量，文件完整内容如下： 

	export HADOOP_HOME=/usr/local/hadoop-3.2.1
	export HADOOP_ROOT_LOGGER=INFO,console
	export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native/
	export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native:$HADOOP_COMMON_LIB_NATIVE_DIR"
	 
	export PATH=$PATH:$HADOOP_HOME/bin:/usr/local/bin

环境变量配置完之后，记得执行`source ~/.bash_profile`命令使环境变量生效。

接下来，还需要修改`%HADOOP_HOME%/etc/hadoop/`目录下面的几个配置文件。

### hadoop-env.sh

	# The java implementation to use.
	export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_241.jdk/Contents/Home
	 
	# Location of Hadoop.
	export HADOOP_HOME=/usr/local/hadoop-3.2.1

### mapred-site.xml

	<configuration>
	        <property>
	                <name>mapreduce.framework.name</name>
	                <value>yarn</value>
	                <final>true</final>
	                <description>The runtime framework for executing MapReduce jobs</description>
	        </property>
	        <property>
	                <name>yarn.app.mapreduce.am.env</name>
	                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop-3.2.1</value>
	        </property>
	        <property>
	                <name>mapreduce.map.env</name>
	                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop-3.2.1</value>
	        </property>
	        <property>
	                <name>mapreduce.reduce.env</name>
	                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop-3.2.1</value>
	        </property>
	</configuration>

### core-site.xml

	<configuration>
	    <!-- 指定 namenode 的通信地址 -->
	    <property>
	        <name>fs.default.name</name>
	        <value>hdfs://localhost:9000</value>
	    </property>
	    <!-- 指定hadoop运行时产生文件的存储路径 -->
	    <property>
	        <name>hadoop.tmp.dir</name>
	        <value>/usr/local/hadoop-3.2.1/temp</value>
	    </property>
	</configuration>

### hdfs-site.xml 

	<configuration>
	        <property>
	                <name>dfs.permissions.enabled</name>
	                <value>false</value>
	        </property>
	        <property>
	                <name>dfs.replication</name>
	                <value>1</value>
	        </property>        
	        <property>
	                <name>dfs.namenode.name.dir</name>
	                <value>/usr/local/hadoop-3.2.1/data/namenode</value>
	        </property>
	        <property>
	                <name>dfs.datanode.data.dir</name>
	                <value>/usr/local/hadoop-3.2.1/data/datanode</value>
	        </property>
	        <property>
	                <name>dfs.namenode.secondary.http-address</name>
	                <value>localhost:9001</value>
	        </property>
	        <property>
	                <name>dfs.webhdfs.enabled</name>
	                <value>true</value>
	        </property>
	        <property>
	                <name>dfs.http.address</name>
	                <value>0.0.0.0:50070</value>
	        </property>
	</configuration>

### yarn-site.xml
	<configuration>
	        <property>
	                <name>yarn.nodemanager.aux-services</name>
	                <value>mapreduce_shuffle</value>
	        </property>
	</configuration>

# 启动Hadoop
## 格式化HDFS

	hdfs namenode -format

## 启动所有节点
一次启动hadoop所有进程：

	start-all.sh
通过`jps`查看启动的进程。

	dengdeyudeMacBook-Pro:hadoop dengdeyu$ jps
	25889 NodeManager
	27009 Jps
	25346 NameNode
	25589 SecondaryNameNode
	25451 DataNode
	25789 ResourceManager

启动成功之后，可以通过<http://localhost:50070>进入hdfs管理页面 ，访问<http://localhost:8088>进入hadoop进程管理页面。

# MapReduce测试

	dengdeyudeMacBook-Pro:hadoop dengdeyu$ hdfs dfs -mkdir /input
	dengdeyudeMacBook-Pro:hadoop dengdeyu$ hdfs dfs -put /usr/local/hadoop-3.2.1/etc/hadoop/hdfs-site.xml /input
	dengdeyudeMacBook-Pro:hadoop dengdeyu$ hadoop jar /usr/local/hadoop-3.2.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep /input /output 'dfs[a-z.]+'
	dengdeyudeMacBook-Pro:hadoop dengdeyu$ hdfs dfs -cat /output/1/part-r-00000
	2020-03-22 19:24:40,387 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
	1	dfs.webhdfs.enabled
	1	dfs.replication
	1	dfs.permissions.enabled
	1	dfs.namenode.secondary.http
	1	dfs.namenode.name.dir
	1	dfs.http.address
	1	dfs.datanode.data.dir
 