> 原文链接：<https://www.cnblogs.com/wenqiangwu/p/3288349.html>

输入`rpm -qa|grep vim`命令, 如果`vim`已经正确安裝,会返回下面的三行代码:

	root@server1 [~]# rpm -qa|grep vim
	vim-enhanced-7.0.109-7.el5
	vim-minimal-7.0.109-7.el5
	vim-common-7.0.109-7.el5

如果少了其中的某一条,比如`vim-enhanced`的,就用命令`yum -y install vim-enhanced`来安裝:

	yum -y install vim-enhanced

如果上面的三条一条都沒有返回, 可以直接用`yum -y install vim*`命令

	yum -y install vim*
 