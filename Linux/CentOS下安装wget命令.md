Linux下使用`wget`命令时提示如下信息：

	-bash: wget: command not found

很显然，问题出现的原因是由于没有安装wget命令，可以通过以下两种方法来安装：

# rpm安装

`wget`的`RPM`包下载地址：<http://www.rpmfind.net/linux/rpm2html/search.php?query=wget(x86-64)>

根据自己的Linux系统版本选择合适的RPM包下载，例如`CentOS7`所需的`wget RPM`包下载链接如下：

	http://www.rpmfind.net/linux/centos/7.6.1810/os/x86_64/Packages/wget-1.14-18.el7.x86_64.rpm

执行如下命令，完成安装：

	rpm -ivh wget-1.14-18.el7.x86_64.rpm

# yum安装

	yum -y install wget