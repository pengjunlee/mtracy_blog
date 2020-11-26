一般情况下我们在使用mac系统过程中下载一些文件、新建一些项目之后，这些文件都会默认是只读状态，对其进行操作时经常会出现`Permission denied`错误，这时我们只需要简单的一句权限设置命令就可以解决。

	sudo chmod -R 777 你要修改文件上层目录的路径

但是我们在对`usr/bin`目录下的文件进行操作的时候往往会出现`operation not permitted`,这是因为一些mac用户在升级系统之后，电脑启用了SIP（`System Integrity Protection`），增加了rootless机制，导致即使在root权限下依然无法修改文件，在必要时候为了能够修改下面的文件，我们只能关闭该保护机制

**1）**重启，过程中按住`command+R`直到出现苹果logo为止，进入保护模式

**2）**电脑进入保护模式后别乱操作，在上方菜单栏找到terminal终端，打开terminal终端，输入

	csrutil disable

**3）**再次重启，即可对`usr/bin`目录下文件进行修改

<font color=red>PS</font>：如果要恢复保护机制，重新进入保护模式，输入

	csrutil enable