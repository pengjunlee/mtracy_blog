CentOS上直接安装部署Kafka请参考：<https://blog.csdn.net/pengjunlee/article/details/103258121>

本篇主要介绍如何使用Docker安装部署Kafka。

# 安装Zookeeper
## 下载JDK安装包
由于在Apache官网下载JDK时需要作安全验证，使用`wget`命令下载经常会失败，所以在这里推荐自己手动到官网下载一下JDK安装包。

官网下载地址：<https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>

在这里，我下载的JDK安装包为`jdk-8u211-linux-x64.tar.gz`。

## 拷贝安装包
本例中，我会将构建镜像所用到的`DockerFile`文件放在`/usr/local/src/dockerfiles/`目录下，若文件夹不存在，请使用如下命令进行创建。

	mkdir /usr/local/src/dockerfiles

将下载好的JDK安装包拷贝到`/usr/local/src/dockerfiles`文件夹下。

	mv jdk-8u211-linux-x64.tar.gz /usr/local/src/dockerfiles/

## 编写DockerFile
在`/usr/local/src/dockerfiles/`目录下创建一个`DockerFile`取名为`dockerfile01`。

	vim /usr/local/src/dockerfiles/dockerfile01

内容如下：

	FROM centos:7.7.1908
	 
	# 标明作者的名字和联系方式
	MAINTAINER pengjunlee pengjunlee@163.com
	 
	# 安装一些常用的命令行工具
	RUN yum -y install vim lsof wget tar bzip2 unzip hostname net-tools rsync man git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel
	 
	# 把JDK安装包复制到 /usr/local/ 目录下
	ADD ./jdk-8u211-linux-x64.tar.gz /usr/local/
	 
	# 设置JDK环境变量
	RUN JAVA_HOME=/usr/local/jdk1.8.0_211 &&\
		sed -i.bak "/^export PATH/i export JAVA_HOME=$JAVA_HOME" /etc/profile &&\
		sed -i "/^export PATH/i PATH=\$PATH:\$JAVA_HOME/bin" /etc/profile
	 
	# 指定 Zookeeper 版本
	ENV ZOOKEEPER_VERSION "3.4.14"
	 
	# 下载 Zookeeper 安装包
	RUN wget http://mirror.olnevhost.net/pub/apache/zookeeper/zookeeper-$ZOOKEEPER_VERSION/zookeeper-$ZOOKEEPER_VERSION.tar.gz -P /usr/local/src
	 
	RUN tar zxvf /usr/local/src/zookeeper*.tar.gz -C /usr/local/
	 
	# 设置 Zookeeper 环境变量
	RUN ZOOKEEPER_HOME=/usr/local/zookeeper-$ZOOKEEPER_VERSION &&\
		sed -i "/^PATH/i export ZOOKEEPER_HOME=$ZOOKEEPER_HOME" /etc/profile &&\
		sed -i 's/PATH=\$PATH/&:\$ZOOKEEPER_HOME\/bin/' /etc/profile
	 
	RUN echo "source /etc/profile" > /usr/local/bin/zKstart.sh &&\
		echo "cp /usr/local/zookeeper-"$ZOOKEEPER_VERSION"/conf/zoo_sample.cfg /usr/local/zookeeper-"$ZOOKEEPER_VERSION"/conf/zoo.cfg" >> /usr/local/bin/zKstart.sh &&\
		echo "/usr/local/zookeeper-$"ZOOKEEPER_VERSION"/bin/zkServer.sh start-foreground" >> /usr/local/bin/zKstart.sh &&\
		chmod a+x /usr/local/bin/zKstart.sh
	 
	EXPOSE 2181
	 
	ENTRYPOINT ["sh", "/usr/local/bin/zKstart.sh"]

## 构建镜像

	[root@localhost dockerfiles]# docker build -t centos_zookeeper:1.0 -f /usr/local/src/dockerfiles/dockerfile01 .
	# .......
	Step 12/12 : ENTRYPOINT sh /usr/local/bin/zKstart.sh
	 ---> Running in d35c10911b90
	 ---> be15c8aeb666
	Removing intermediate container d35c10911b90
	Successfully built be15c8aeb666
	[root@localhost dockerfiles]# docker images
	REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
	centos_zookeeper                 1.0                 be15c8aeb666        20 seconds ago      910 MB

## 使用镜像启动ZK

	[root@localhost dockerfiles]# docker run -itd --name zookeeper -h zookeeper -p2181:2181 centos_zookeeper:1.0 bash
	8fea607f58502051ca2b2db34aa9e34e76a12d00f145526a221c6c8c217b2e33
	[root@localhost dockerfiles]# docker ps
	CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                    NAMES
	8fea607f5850        centos_zookeeper:1.0     "sh /usr/local/bin..."   4 seconds ago       Up 3 seconds        0.0.0.0:2181->2181/tcp   zookeeper

# 安装Kafka
## 编写DockerFile
在`/usr/local/src/dockerfiles/`目录下创建一个`DockerFile`取名为`dockerfile02`。

内容如下：

	FROM centos:7.7.1908
	 
	# 标明作者的名字和联系方式
	MAINTAINER pengjunlee pengjunlee@163.com
	 
	# 安装一些常用的命令行工具
	RUN yum -y install vim lsof wget tar bzip2 unzip hostname net-tools rsync man git make automake cmake patch logrotate python-devel libpng-devel libjpeg-devel nc
	 
	# 把JDK安装包复制到 /usr/local/ 目录下
	ADD ./jdk-8u211-linux-x64.tar.gz /usr/local/
	 
	# 设置JDK环境变量
	RUN JAVA_HOME=/usr/local/jdk1.8.0_211 &&\
		sed -i.bak "/^export PATH/i export JAVA_HOME=$JAVA_HOME" /etc/profile &&\
		sed -i "/^export PATH/i PATH=\$PATH:\$JAVA_HOME/bin" /etc/profile
	 
	# 更换YUM源
	RUN mkdir /etc/yum.repos.d/backup &&\
		mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/backup/ &&\
		curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
	 
	# 指定 Kafka 版本
	ENV KAFKA_VERSION "2.4.0"
	 
	# 下载 Kafka 安装包
	RUN wget http://apache.fayea.com/kafka/$KAFKA_VERSION/kafka_2.13-$KAFKA_VERSION.tgz -P /usr/local/src
	 
	RUN tar zxvf /usr/local/src/kafka_*.tgz -C /usr/local/ &&\
		sed -i 's/num.partitions.*$/num.partitions=3/g' /usr/local/kafka_2.13-$KAFKA_VERSION/config/server.properties
	 
	RUN echo "source /etc/profile" > /usr/local/bin/kafkastart.sh &&\
		echo "cd /usr/local/kafka_2.13-"$KAFKA_VERSION >> /usr/local/bin/kafkastart.sh &&\
		echo "sed -i 's%zookeeper.connect=.*$%zookeeper.connect=zookeeper:2181%g'  /usr/local/kafka_2.13-"$KAFKA_VERSION"/config/server.properties" >> /usr/local/bin/kafkastart.sh &&\
		#echo "[ ! -z $""BROKER_ID"" ] && sed -i 's%broker.id=.*$%broker.id='$""BROKER_ID'""%g'  /usr/local/kafka_2.13-"$KAFKA_VERSION"/config/server.properties" >> /usr/local/bin/kafkastart.sh &&\
		#echo "sed -i '/^#listeners=/a listeners=\$KAFKA_HOST:\$KAFKA_PORT'  /usr/local/kafka_2.13-"$KAFKA_VERSION"/config/server.properties" >> /usr/local/bin/kafkastart.sh &&\
		echo "bin/kafka-server-start.sh config/server.properties" >> /usr/local/bin/kafkastart.sh &&\
		chmod a+x /usr/local/bin/kafkastart.sh
	 
	EXPOSE 9092
	 
	WORKDIR /usr/local/kafka_2.13-$KAFKA_VERSION
	 
	ENTRYPOINT ["sh", "/usr/local/bin/kafkastart.sh"]

## 构建镜像

	[root@localhost dockerfiles]# docker build -t centos_kafka:1.0 -f /usr/local/src/dockerfiles/dockerfile02 .
	# ......
	Step 13/13 : ENTRYPOINT sh /usr/local/bin/kafkastart.sh
	 ---> Running in c10b98a7c45b
	 ---> 453922749684
	Removing intermediate container c10b98a7c45b
	Successfully built 453922749684
	[root@localhost dockerfiles]# docker images
	REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
	centos_kafka                     1.0                 453922749684        3 minutes ago       941 MB

## 使用镜像启动Kafka

	[root@localhost dockerfiles]# docker run -itd --name kafka -h kafka -p9092:9092 --link zookeeper centos_kafka:1.0 /bin/bash
	d67efa63c8c22133164091aae82c8177b28f886600622f225177cfa378594066
	[root@localhost dockerfiles]# docker ps
	CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                    NAMES
	d67efa63c8c2        centos_kafka:1.0         "sh /usr/local/bin..."   4 seconds ago       Up 3 seconds        0.0.0.0:9092->9092/tcp   kafka
	2597c1050898        centos_zookeeper:1.0     "sh /usr/local/bin..."   52 seconds ago      Up 52 seconds       0.0.0.0:2181->2181/tcp   zookeeper
 
	# 接下来进入kafka容器，测试一下创建topic
	[root@localhost dockerfiles]# docker exec -it kafka bash
	[root@kafka kafka_2.13-2.4.0]# bin/kafka-topics.sh --create --topic topic1 --zookeeper zookeeper:2181 --partitions 3 --replication-factor 1
	/usr/local/kafka_2.13-2.4.0/bin/kafka-run-class.sh: line 309: exec: java: not found
	[root@kafka kafka_2.13-2.4.0]# source /etc/profile
	[root@kafka kafka_2.13-2.4.0]# bin/kafka-topics.sh --create --topic topic1 --zookeeper zookeeper:2181 --partitions 3 --replication-factor 1
	Created topic topic1.
	[root@kafka kafka_2.13-2.4.0]# bin/kafka-topics.sh --describe --zookeeper zookeeper:2181 --topic topic1 
	Topic: topic1	PartitionCount: 3	ReplicationFactor: 1	Configs: 
		Topic: topic1	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
		Topic: topic1	Partition: 1	Leader: 0	Replicas: 0	Isr: 0
		Topic: topic1	Partition: 2	Leader: 0	Replicas: 0	Isr: 0

## 消息测试

	# 启动一个 Console 消费者，消费 topic1 主题
	[root@kafka kafka_2.13-2.4.0]# source /etc/profile
	[root@kafka kafka_2.13-2.4.0]# bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic topic1

打开一个新的命令行窗口并启动一个`Console`生产者，开始创建消息。 

	# 启动一个 Console 生产者，向 topic1 主题写入消息
	[root@localhost ~]# docker exec -it kafka bash
	[root@kafka kafka_2.13-2.4.0]# source /etc/profile
	[root@kafka kafka_2.13-2.4.0]# bin/kafka-console-producer.sh --broker-list kafka:9092 --topic topic1
	>hello docker !

在`Console`消费者的命令行窗口中若能同步看到`Console`生产者创建的消息则证明Kafka安装成功。

	[root@kafka kafka_2.13-2.4.0]# bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic topic1
	hello docker !
 