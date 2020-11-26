> 原文链接：<https://www.cnblogs.com/silent2012/p/4682770.html>

CentOS7默认使用的是`firewall`作为防火墙，使用`iptables`必须重新设置一下

# 直接关闭防火墙

	systemctl stop firewalld.service # 停止firewall
	systemctl disable firewalld.service # 禁止firewall开机启动

# 设置iptables service

	yum -y install iptables-services

如果要修改防火墙配置，如增加防火墙端口`3306`

	vi /etc/sysconfig/iptables

增加规则

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

保存退出后

	systemctl restart iptables.service # 重启防火墙使配置生效
	systemctl enable iptables.service # 设置防火墙开机启动

最后重启系统使设置生效即可。
 