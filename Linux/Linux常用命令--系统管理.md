# write
**命令用途**：向登录系统的其他用户发送信息，以`Ctrl+D`保存结束。

**命令语法**：`write[用户名]`

**用法示例**：

	//向pengjunlee用户发送信息
	write pengjunlee

> 注：信息输入有误时可以使用`Ctrl+Backspace`回退删除。  

# wall
**命令用途**：`发广播信息`

**命令语法**：`wall[message]`

**用法示例**：

	//广播信息：Hello Linux
	wall Hello Linux

# ping
**命令用途**：`测试主机之间网络的连通性`

**命令语法**：`ping[选项][IP地址]`
 
**常用选项**：

	-c：指定请求发送次数； 
<br/>
**用法示例**：

	//测试与IP地址为192.168.12.1的主机之间的连通性，请求10次
	ping -c 10 192.168.12.1

# ifconfig
**命令用途**：`查看和设置网卡信息`

**命令语法**：`ifconfig[网卡名称][IP地址]`

**用法示例**：

	//设置eth0网卡的IP地址为192.168.137.135
	ifconfig eth0 192.168.137.135

# mail
**命令用途**：查看发送电子邮件。

**命令语法**：mail[用户名]

**用法示例**：

	//向pengjunlee用户发送邮件
	mail pengjunlee

> 注：邮件内容输入有误时可以使用`Ctrl+Backspace`回退删除。  

# last
**命令用途**：列出目前与过去登录系统的用户信息。

**命令语法**：last 

**用法示例**：

	//列出目前与过去登录系统的用户信息
	last

# lastlog
**命令用途**：`显示系统中所有用户最近一次登录信息`

**命令语法**：`lastlog[参数][用户名]`

**常用选项**：

	-c：指定用户；  
<br/>
**用法示例**：

	//查看系统中所有用户最近一次登录信息
	lastlog
	//查看用户pengjunlee最近一次的登录信息
	lastlog -u pengjunlee

# traceroute
**命令用途**：`追踪数据包在网络上传输时的全部路径`

**命令语法**：`traceroute[主机地址]`

**用法示例**：

	//显示数据包到主机www.baidu.com之间的路径
	traceroute www.baidu.com

# netstat
**命令用途**：`显示网络相关信息`

**命令语法**：`netstat[选项]`

**常用选项**：

	-t：TCP协议；
	-u：UDP协议；
	-l：监听；
	-r：路由；
	-n：显示IP地址和端口号；
	-a：显示所有连接中的socket； 
<br/>
**用法示例**：

	//查看本机监听的端口
	netstat -tlun
	//查看本机所有的网络连接
	netstat -an
	//查看本机路由表
	netstat -rn

# setup
**命令用途**：`配置网络工具`

**命令语法**：`setup`

	//打开setup网络配置工具
	setup

# shutdown
**命令用途**：`关机或重启`

**命令语法**：`shutdown[选项][时间]`

**常用选项**：

	-c：取消前一个关机命令；
	-h：关机；
	-r：关机之后重新启动； 
<br/>
**用法示例**：

	//立即关机
	shutdown -h now
	//立即重启
	shutdown -r now
	//5分钟后关机，同时送出警告信息给登入用户
	shutdown +5 "System will shutdown after 5 minutes"

> 注：
> 1. 以下这些命令同样可以实现关机功能：`halt`、`poweroff`、`init 0`。
> 2. 以下这些命令同样可以实现重启功能：`reboot`、`init 6`。
> 3. 查看启动运行级别：`cat /etc/inittab`或`runlevel`。 

<table border="1" cellpadding="0" cellspacing="0" style="width:442px;"><tbody><tr><td colspan="2" style="text-align:center;"><span style="color:#000099;"><strong>Linux系统运行级别</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>0</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>关机</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>1</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>单用户</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>2</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>不完全多用户，不含NFS服务</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>3</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>完全多用户</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>4</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>未分配</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>5</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>图形界面</strong></span></td></tr><tr><td style="text-align:center;width:145px;"><span style="color:#cc0000;"><strong>6</strong></span></td><td style="text-align:center;width:297px;"><span style="color:#cc0000;"><strong>重启</strong></span></td></tr></tbody></table>

# logout
**命令用途**：`退出登录`

**命令语法**：`logout`

**用法示例**：

	//退出登录
	logout
 