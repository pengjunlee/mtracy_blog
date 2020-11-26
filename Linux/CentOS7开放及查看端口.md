> 原文链接：<https://www.cnblogs.com/heqiuyong/p/10460150.html>

# 开放端口

	firewall-cmd --zone=public --add-port=5672/tcp --permanent   # 开放5672端口
	firewall-cmd --zone=public --remove-port=5672/tcp --permanent  #关闭5672端口
	firewall-cmd --reload   # 配置立即生效

# 查看防火墙所有开放的端口

	firewall-cmd --zone=public --list-ports

# 关闭防火墙
如果要开放的端口太多，嫌麻烦，可以关闭防火墙，安全性自行评估

	systemctl stop firewalld.service

# 查看防火墙状态

	firewall-cmd --state

# 查看监听的端口

	netstat -lnpt

<font color=red>PS</font>:centos7默认没有`netstat`命令，需要安装`net-tools`工具，`yum install -y net-tools`

# 检查端口被哪个进程占用

	netstat -lnpt |grep 5672

# 查看进程的详细信息

	ps 6832

# 中止进程

	kill -9 6832
 