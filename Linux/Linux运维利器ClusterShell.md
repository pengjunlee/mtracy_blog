> 原文链接：<https://www.jianshu.com/p/9f546be2a21b>

# 优点
它的优点很多，我最看重的有三点：

- 安装简单：在CentOS 7下一条命令搞定。
- 配置简单：我们只需要配置管理服务器可以通过SSH免密登录其他客户端。
- 使用方便：ClusterShell指令只有简单的2~3条，其他就像在本地操作一样。

# 安装

	sudo yum install clustershell

# 配置
`ClusterShell`的配置文件都位于`/etc/clustershell`中。我只配置了`groups`文件，为了方便，直接编辑`/etc/clustershell/groups.d/`下的`local.cfg`文件：

	sudo vi /etc/clustershell/groups.d/local.cfg

设置了一个群组hadoop：

	hadoop: master secondary slave[1-3]
	all: master secondary slave[1-3]

简单说明一下：

- master： 我的hadoop master NameNode主机
- secondary： 我的hadoop secondary NameNode机器
- slave1~3： 数据节点3个

# 命令行介绍

	-b : 相同输出结果合并
	-w : 指定节点
	-a : 所有节点
	-g : 指定组
	--copy : 群发文件

## 查看节点系统配置信息

	clush -a echo $HADOOP_HOME 

还可以将输出信息合并，看起来更一目了然些：

	clush -b -a echo $HADOOP_HOME

## 分发文件 

	clush -g hadoop --copy /opt/hadoop/etc/hadoop/hdfs-site.xml