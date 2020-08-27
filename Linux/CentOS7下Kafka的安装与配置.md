# 环境准备
## 安装JDK
在安装Kafka之前需要先安装JDK，JDK的安装与配置，请参考文章：<https://blog.csdn.net/pengjunlee/article/details/53932094>

## 下载安装包
官网下载：<http://kafka.apache.org/downloads>

# 单节点安装
Kafka当前的稳定版本是`2.4.0`，下载其二进制安装包`kafka_2.13-2.4.0.tgz`。

	# 下载 kafka_2.13-2.4.0.tgz 安装包到 /usr/local/src/ 目录
	[root@localhost ~]# wget -P /usr/local/src/ http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.4.0/kafka_2.13-2.4.0.tgz
	# 将 kafka_2.13-2.4.0.tgz 安装包解压到 /usr/local/ 目录
	[root@localhost ~]# tar -zxvf /usr/local/src/kafka_2.13-2.4.0.tgz -C /usr/local/
	# 切换至 Kafka 解压的 /bin 目录
	[root@localhost kafka_2.13-2.4.0]# cd /usr/local/kafka_2.13-2.4.0/bin/

## 启动Zookeeper
在启动Kafka之前需要先启动`Zookeeper`，在此直接使用Kafka安装包中内嵌的`Zookeeper`。

	# 查看 ./zookeeper-server-start.sh 命令的用法
	[root@localhost bin]# ./zookeeper-server-start.sh 
	USAGE: ./zookeeper-server-start.sh [-daemon] zookeeper.properties
	# 启动 Zookeeper
	[root@localhost bin]# ./zookeeper-server-start.sh ../config/zookeeper.properties

## 启动Kafka

	# 查看 ./kafka-server-start.sh 命令的用法
	[root@localhost bin]# ./kafka-server-start.sh
	USAGE: ./kafka-server-start.sh [-daemon] server.properties [--override property=value]*
	# 启动 Kafka
	[root@localhost bin]# ./kafka-server-start.sh ../config/server.properties

## 验证是否安装成功

	# 首先创建一个 Topic ，其名称为 topic1
	[root@localhost bin]# ./kafka-topics.sh --zookeeper localhost:2181 --create --topic topic1 --partitions 3 --replication-factor 1
	Created topic topic1.
	# 查看 topic1 的配置信息
	[root@localhost bin]# ./kafka-topics.sh --zookeeper localhost:2181 --describe --topic topic1
	Topic: topic1	PartitionCount: 3	ReplicationFactor: 1	Configs: 
		Topic: topic1	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
		Topic: topic1	Partition: 1	Leader: 0	Replicas: 0	Isr: 0
		Topic: topic1	Partition: 2	Leader: 0	Replicas: 0	Isr: 0

Kafka提供了一个`Console`消费者，它会将消费的消息内容打印出来。接下来，我们使用它来验证安装好的Kafka是否能正常工作。

	# 启动一个 Console 消费者，消费 topic1 主题
	[root@localhost bin]# ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic1

打开一个新的命令行窗口并启动一个`Console`生产者，开始创建消息。

	# 启动一个 Console 生产者，向 topic1 主题写入消息
	[root@localhost bin]# ./kafka-console-producer.sh --broker-list localhost:9092 --topic topic1
	# 创建两条消息内容
	>hello kafka
	>bye!


在`Console`消费者的命令行窗口中若能同步看到`Console`生产者创建的消息则证明Kafka安装成功。

	[root@localhost bin]# ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic1
	# 在 Console 消费者的命令行窗口中同步显示 Console 生产者创建的消息
	hello kafka
	bye!

## 使用已部署好的Zookeeper集群
在实际项目中，我们的服务器上通常都已经安装好了Zookeeper集群，在这种情况下，我们一般不会直接使用的Kafka自带的Zookeeper来启动Kafka服务，而是复用已经部署好的Zookeeper集群。此时，需要修改一下`config/server.properties`配置。

	zookeeper.connect=172.16.250.233:2181,172.16.250.234:2181,172.16.250.237:2181

# 使用ClusterShell部署Kafka集群
3个节点服务器信息如下：

|IP Address|User Name|
|-|-|
|172.16.250.233|root|
|172.16.250.234|root|
|172.16.250.237|root|

## SSH免密码登录配置
在`Kafka`集群中的各个节点之间需要使用`SSH`频繁地进行通信，为了避免每次的通信都要求输入密码，需要对各个节点进行`SSH`免密码登录配置。

### 开启sshd秘钥认证
在进行SSH免密码登录配置之前，需要先开启`sshd`秘钥认证：编辑每一台机器的`/etc/ssh/sshd_config`文件，去掉下面这3行前的`#`注释。

	# RSAAuthentication yes
	# PubkeyAuthentication yes
	# AuthorizedKeysFile      .ssh/authorized_keys

修改完成后保存，并执行以下命令重启`sshd`服务使修改生效。

	systemctl restart  sshd              # 重启 sshd 服务

### 创建免密码登录账户
由于`Kafka`集群中的各节点默认会使用当前的账号SSH免密码登录其它节点，所以需要在每个节点中创建一个相同的供 `Kafka`集群专用的账户，本例中为了操作方便，直接使用的`root`用户 。

### 生成公钥和私钥
执行`ssh-keygen -t rsa`命令，生成用来SSH免密码登录的公钥和私钥。

	[root@localhost ~]# ssh-keygen -t rsa
	Generating public/private rsa key pair.
	Enter file in which to save the key (/root/.ssh/id_rsa): 
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /root/.ssh/id_rsa.
	Your public key has been saved in /root/.ssh/id_rsa.pub.
	The key fingerprint is:
	88:e5:23:fb:c9:52:e5:fc:f0:64:c4:a8:a4:d6:8e:16 root@localhost.localdomain
	The key's randomart image is:
	+--[ RSA 2048]----+
	|                 |
	|                 |
	|      .  o       |
	|     +..o o      |
	|    o++=S.       |
	|    Eo+.+ o      |
	|   ..=   *       |
	|    +o..  o      |
	|   . .+          |
	+-----------------+

上述秘钥生成过程中，无需指定秘钥存放目录和口令密码，直接回车，命令执行完毕后会在`root`账户的家目录中（`/root/.ssh`）生成两个文件：

	[root@localhost ~]# ls /root/.ssh
	id_rsa  id_rsa.pub  known_hosts

- id_rsa: 私钥
- id_rsa.pub:公钥

### 将公钥导入到认证文件（172.16.250.233）
秘钥生成之后，在`172.16.250.233`主机上执行以下命令将每台机器的公钥都拷贝到`172.16.250.233`主机的认证文件 `authorized_keys`中。

	[root@localhost ~]# cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
	[root@localhost ~]# ssh root@172.16.250.234 cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
	root@172.16.250.234's password:      # 键入该服务器 root 用户的密码
	[root@localhost ~]# ssh root@172.16.250.237 cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
	The authenticity of host '172.16.250.237 (172.16.250.237)' can't be established.
	ECDSA key fingerprint is 41:f9:3e:bf:1c:58:75:a6:09:76:de:7a:bd:eb:00:00.
	Are you sure you want to continue connecting (yes/no)? yes   
	Warning: Permanently added '172.16.250.237' (ECDSA) to the list of known hosts.
	root@172.16.250.237's password:      # 键入该服务器 root 用户的密码
	[root@localhost ~]# 

查看`authorized_keys`文件内容如下：

	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDctNy/ig2OIQ+E1gQSgmjNwlflvNT3ssDl2ho5tRDuo9YLCyDwcygw7I6jQzKOv5kBqhbl8lis7N0Ymk736xIwYEvZBc1ejFU8vfL8GgvX8+ltT7d/QSUZKnp14JmVZj0wekAOLGsnlhYp1TQGtGLMq2F3l4e1ltLaPcsYY9tEN6meihKD6mivTHVR+K+v3yP6mpWCRiIawaqYVjJj27FRBKQwvH5GGUUrC0oAlKRZhvS7Vr3GcOf1kJOdBr5VTYZS6q/8Hwx0XDnNsdlwiEM/W7KbxJpjwrV3SW/CcZoMUbUBwDC5IHyClmMyVnKzgE27zKqRl4y7FqUY2AM1ZxLp root@localhost.localdomain
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpjVIZwi3yQXgQXYzjQbfzt3kb+FJb1xglTzgyQGAk8g8Qv4EmAiqo7l5wapDhnsTUifu7t0WJYd1VY2UlD4+xqd0RIimVSm4OLoi8yF9oSoffaz+JWEJOva9F8GRjByz7Uf41e4mZut3d4RZjVqoCKh7LkPcoBUZ4ZSNiLvEbHQSWM/eDOyGhRiHTe+QYMuf88rZ6xhjO7Z9/iNYhOkflPa6GJo5eNJi2W1dI1pYYY53sgoMHbkTLvlrCnjA1mf7iTCC8mIb9gu3DKbss/Tqp/tnYBjtvIWsD7TeqOHeCno3rvJaJCU1ynvLAit5OJkHHXyq78YuOxxvjmzDJvpkr root@localhost.localdomain
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbLY2wI+VvKAp1yht/q64GnPNQUIerWiuaZx8UHZHTgz4R8VtMS1WC5RvuYqFs2a6IwoUlTeAG06MTPjwPjp6vAiuTm+EqGLeBRJpgpI3ULZ2WJxpqMAJJjR/qeTWCM1BxsKHKctKMvbwIb10v436OMR86CX47cmcvcx/gOw/DJiNqnE9oK8mBvRbvgzh9OVPwQQk13rdROaUzBCEnFeGVMd2oxaQ85zQx6cCyF3kCAzmNkjfiNn9+wp3z/oqwYSkcnfXrRiEQPJ//ZHk7p/ZR6oorDUdCzJKuddOleEgX4V6eqPhJolHF3/8bF0LzB/zcxNNNiwxZiAgxSI8gwLPl root@localhost.localdomain

修改其中各个服务器的IP地址：

	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDctNy/ig2OIQ+E1gQSgmjNwlflvNT3ssDl2ho5tRDuo9YLCyDwcygw7I6jQzKOv5kBqhbl8lis7N0Ymk736xIwYEvZBc1ejFU8vfL8GgvX8+ltT7d/QSUZKnp14JmVZj0wekAOLGsnlhYp1TQGtGLMq2F3l4e1ltLaPcsYY9tEN6meihKD6mivTHVR+K+v3yP6mpWCRiIawaqYVjJj27FRBKQwvH5GGUUrC0oAlKRZhvS7Vr3GcOf1kJOdBr5VTYZS6q/8Hwx0XDnNsdlwiEM/W7KbxJpjwrV3SW/CcZoMUbUBwDC5IHyClmMyVnKzgE27zKqRl4y7FqUY2AM1ZxLp root@172.16.250.233
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpjVIZwi3yQXgQXYzjQbfzt3kb+FJb1xglTzgyQGAk8g8Qv4EmAiqo7l5wapDhnsTUifu7t0WJYd1VY2UlD4+xqd0RIimVSm4OLoi8yF9oSoffaz+JWEJOva9F8GRjByz7Uf41e4mZut3d4RZjVqoCKh7LkPcoBUZ4ZSNiLvEbHQSWM/eDOyGhRiHTe+QYMuf88rZ6xhjO7Z9/iNYhOkflPa6GJo5eNJi2W1dI1pYYY53sgoMHbkTLvlrCnjA1mf7iTCC8mIb9gu3DKbss/Tqp/tnYBjtvIWsD7TeqOHeCno3rvJaJCU1ynvLAit5OJkHHXyq78YuOxxvjmzDJvpkr root@172.16.250.234
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbLY2wI+VvKAp1yht/q64GnPNQUIerWiuaZx8UHZHTgz4R8VtMS1WC5RvuYqFs2a6IwoUlTeAG06MTPjwPjp6vAiuTm+EqGLeBRJpgpI3ULZ2WJxpqMAJJjR/qeTWCM1BxsKHKctKMvbwIb10v436OMR86CX47cmcvcx/gOw/DJiNqnE9oK8mBvRbvgzh9OVPwQQk13rdROaUzBCEnFeGVMd2oxaQ85zQx6cCyF3kCAzmNkjfiNn9+wp3z/oqwYSkcnfXrRiEQPJ//ZHk7p/ZR6oorDUdCzJKuddOleEgX4V6eqPhJolHF3/8bF0LzB/zcxNNNiwxZiAgxSI8gwLPl root@172.16.250.237

### 设置认证文件访问权限（172.16.250.233）
在`172.16.250.233`主机上执行如下命令，对认证文件的操作权限进行设置：

	[root@localhost ~]# chmod 700 /root/.ssh
	[root@localhost ~]# chmod 600 /root/.ssh/authorized_keys

### 将认证文件复制到其他主机
在`172.16.250.233`主机上执行以下命令将生成的`authorized_keys`文件复制到`172.16.250.234`和 `172.16.250.237`主机上 。

	[root@localhost ~]# scp /root/.ssh/authorized_keys root@172.16.250.234:/root/.ssh/authorized_keys
	root@172.16.250.234's password: 
	authorized_keys                                                                                                                                                                                100% 1203     1.2KB/s   00:00    
	[root@localhost ~]# scp /root/.ssh/authorized_keys root@172.16.250.237:/root/.ssh/authorized_keys
	root@172.16.250.237's password: 
	authorized_keys

复制完成后，查看known_hosts中的主机列表，发现此时`172.16.250.233`主机的`known_hosts`中仅有`172.16.250.234`和`172.16.250.237`两台主机的信息，缺少`172.16.250.233`主机自身的信息。

	172.16.250.234 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLCwNXd5foXYczn7wX3xzQ2hAiocLGOegUQrH0cItbjc8Tz2V0f6lCBRXhujblWp0M2uG+dDCGZoIiF3vH2Os8w=
	172.16.250.237 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL6CsqwCWhrdjYuTSscBvBV2zw3Cc65JCKvipalOW2qXEtb1YB+PUlruB78Y7NBMFFf2Yh/jePMIofEQigOhCyY=

所以需要ssh免密登录一次将自身的主机信息添加到known_hosts列表中。

	[root@localhost ~]# ssh 172.16.250.233

再次查看`172.16.250.233`主机的`known_hosts`主机列表，`172.16.250.233`主机自身的信息也被添加进来了。

	172.16.250.234 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLCwNXd5foXYczn7wX3xzQ2hAiocLGOegUQrH0cItbjc8Tz2V0f6lCBRXhujblWp0M2uG+dDCGZoIiF3vH2Os8w=
	172.16.250.237 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL6CsqwCWhrdjYuTSscBvBV2zw3Cc65JCKvipalOW2qXEtb1YB+PUlruB78Y7NBMFFf2Yh/jePMIofEQigOhCyY=
	172.16.250.233 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLKwyvKdYnXzYxbV2hoTaj3etYr5YTUvL0BNcdqOZfK5TIfg3yJdSKYC8AM6OmKYQHs8RxKjuaf47cPYs1OTURk=

在`172.16.250.233`主机上执行以下命令将生成的`known_hosts`文件也复制到`172.16.250.234`和 `172.16.250.237`主机上 。

	[root@localhost ~]# scp /root/.ssh/known_hosts root@172.16.250.234:/root/.ssh/known_hosts 
	known_hosts                                                                                                                                                                                    100%  528   503.0KB/s   00:00    
	[root@localhost ~]# scp /root/.ssh/known_hosts root@172.16.250.237:/root/.ssh/known_hosts 
	known_hosts

### 设置认证文件访问权限（234和237）
在`172.16.250.234`和`172.16.250.237`两台主机上分别执行如下命令，修改认证文件的访问权限。

	[root@localhost ~]# chmod 700 /root/.ssh
	[root@localhost ~]# chmod 600 /root/.ssh/authorized_keys

### SSH免密码登录测试
在`172.16.250.237`主机上执行`ssh 172.16.250.234`命令就能够免密码登录`172.16.250.234`主机了。

	[root@localhost ~]# ssh 172.16.250.234
	Last login: Mon Dec 23 17:12:07 2019 from 172.16.250.233

SSH免密码登录详细配置，请参考文章：<https://blog.csdn.net/pengjunlee/article/details/80919833>

## 安装ClusterShell
在`172.16.250.233`主机上执行如下命令来安装ClusterShell。

	yum -y install clustershell

若安装不成功，可以自己编译安装：<https://blog.csdn.net/pengjunlee/article/details/103670636>

ClusterShell安装完成后 ，编辑`/etc/clustershell/groups`文件。

	[root@localhost clustershell-1.7.3]# vim /etc/clustershell/groups

在`/etc/clustershell/groups`文件中添加一个`Kafka`组。

	kafka: 172.16.250.233 172.16.250.234 172.16.250.237

## 安装并启动Zookeeper集群
Zookeeper集群的详细安装配置过程，请参考文章：<https://blog.csdn.net/pengjunlee/article/details/81637024>

## 安装并配置Kafka集群
### 下载安装包
在`172.16.250.233`主机上执行如下命令下载Kafka的安装包。 

	# 下载 kafka_2.13-2.4.0.tgz 安装包到 /usr/local/src/ 目录
	[root@localhost ~]# wget -P /usr/local/src/ http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.4.0/kafka_2.13-2.4.0.tgz
	# 将 kafka_2.13-2.4.0.tgz 安装包解压到 /usr/local/ 目录
	[root@localhost ~]# tar -zxvf /usr/local/src/kafka_2.13-2.4.0.tgz -C /usr/local/
	# 切换至 Kafka 解压的 /bin 目录
	[root@localhost kafka_2.13-2.4.0]# cd /usr/local/kafka_2.13-2.4.0/bin/

### 集群配置
使用`vim config/server.properties`命令编辑`config/server.properties`配置，主要修改如下5个配置项。

	# Kafka 服务器的唯一ID
	broker.id=0
	# Kafka 的监听地址
	listeners=PLAINTEXT://172.16.250.233:9092
	# Kafka 供生产者和消费者访问的地址
	advertised.listeners=PLAINTEXT://172.16.250.233:9092
	# Zookeeper 集群地址
	zookeeper.connect=172.16.250.233:2181,172.16.250.234:2181,172.16.250.237:2181
	# 消息数据保存目录
	log.dirs=/tmp/kafka-logs

接下来将`172.16.250.233`主机上的`/usr/local/kafka_2.13-2.4.0/`目录复制到另外两台机器上。

	[root@localhost kafka_2.13-2.4.0]# clush -g kafka --copy /usr/local/kafka_2.13-2.4.0/

使用`vim config/server.properties`命令，将`172.16.250.234`和`172.16.250.237`两台服务器上`Kafka`的 `broker.id`分别改为`1`和`2`，并将`listeners`和`advertised.listeners`两个配置项中的Host改为它们自己的IP。

### 启动集群
我们可以使用`ClusterShell`来启动集群，若`ClusterShell`不好使就自己挨个启动一下： 

	[root@localhost ~]# clush -g kafka /usr/local/kafka_2.13-2.4.0/bin/kafka-server-start.sh -daemon /usr/local/kafka_2.13-2.4.0/config/server.properties

确认集群启动成功之后创建一个topic进行测试。 

	[root@localhost local]# /usr/local/kafka_2.13-2.4.0/bin/kafka-topics.sh --zookeeper 172.16.250.233:2181 --create --topic topic1 --partitions 3 --replication-factor 2
	Created topic topic1.
	[root@localhost local]# /usr/local/kafka_2.13-2.4.0/bin/kafka-topics.sh --zookeeper 172.16.250.233:2181 --describe --topic topic1
	Topic: topic1	PartitionCount: 3	ReplicationFactor: 2	Configs: 
		Topic: topic1	Partition: 0	Leader: 1	Replicas: 1,2	Isr: 1,2
		Topic: topic1	Partition: 1	Leader: 2	Replicas: 2,0	Isr: 2,0
		Topic: topic1	Partition: 2	Leader: 0	Replicas: 0,1	Isr: 0,1

接下来，我们启动一个`Console`消费者。

	# 启动一个 Console 消费者，消费 topic1 主题
	[root@localhost bin]# ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic1

再打开一个新的命令行窗口并启动一个`Console`生产者创建一条消息。

	/usr/local/kafka_2.13-2.4.0/bin/kafka-console-producer.sh --broker-list 172.16.250.233:9092,172.16.250.234:9092,172.16.250.237:9092 --topic topic1
	>hello kafka

在`Console`消费者的命令行窗口中若能同步看到`Console`生产者创建的消息则证明Kafka安装成功。