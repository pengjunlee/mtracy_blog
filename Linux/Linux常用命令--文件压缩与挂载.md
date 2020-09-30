# gzip
**命令用途**：`压缩或解压缩文件`

**压缩格式**：`.gz`

**命令语法**：`gzip[选项][文件]`

**常用选项**：

	-d：解压文件；
	-r：递归处理；
	-v：显示执行过程；
	-l：列出压缩文件的相关信息；
<br/>
**用法示例**：

	//递归压缩test/logs/ 目录下的所有文件，该目录下的所有文件都会被压缩成.gz格式
	gzip -rv /test/logs/ 
	//查看install.log.gz的压缩信息
	gzip -l install.log.gz
	//递归解压test/logs/ 目录下的所有.gz格式的文件
	gzip -rdv /test/logs/

# gunzip
**命令用途**：等价于`gzip -d`，用来解压缩文件

**压缩格式**：`.gz`

**命令语法**：`gunzip[选项][文件]`

**常用选项**：

	-r：递归处理；
	-v：显示执行过程；
	-l：列出压缩文件的相关信息； 
<br/>
**用法示例**：

	//递归解压test/logs/ 目录下的所有.gz格式的文件
	gunzip -rv /test/logs/
	//查看install.log.gz的压缩信息
	gunzip -l install.log.gz

# tar
**命令用途**：`打包备份文件或目录`

**压缩格式**：`.tar`

**命令语法**：`tar[选项][备份文件名][文件或目录]`

**常用选项**：

	-c：打包；
	-v：显示执行过程；
	-f：指定备份文件名；
	-z：以gzip格式压缩或解压文件；
	-x：解包；
	-j：以bzip2格式压缩或解压文件； 
<br/>
**用法示例**：

	//将readme.txt文件进行打包并压缩备份，备份文件名readme.txt.tar.gz
	tar -czvf readme.txt.tar.gz readme.txt
	//还原readme.txt.tar.gz中备份的文件
	tar -xzvf readme.txt.tar.gz 
	//将logs目录进行压缩备份，备份名称logs.tar.gz
	tar -czvf logs.tar.gz logs/
	//将readme.txt文件进行打包并压缩备份，备份文件名readme.txt.tar.bz2
	tar -cjvf readme.txt.tar.bz2 readme.txt
	//还原readme.txt.tar.bz2中备份的文件
	tar -xjvf readme.txt.tar.bz2

# zip
**命令用途**：`压缩备份文件或目录`

**压缩格式**：`.zip`

**命令语法**：`zip[选项][备份文件名][文件或目录]`

**常用选项**：

	-r：压缩目录；
	-v：显示执行过程； 
<br/>
**用法示例**：

	//将install.log文件进行压缩备份，备份名称install.log.zip
	zip -v install.log.zip install.log
	//将logs目录进行压缩备份，备份名称logs.zip
	zip -rv logs.zip logs/

# unzip
**命令用途**：`解压缩文件`

**压缩格式**：`.zip`

**命令语法**：`unzip[选项][压缩文件]`

**常用选项**：

	-d：指定解压目录； 
<br/>
**用法示例**：

	//还原logs.zip中备份的文件解压到tmp目录下
	unzip -d /tmp logs.zip 

# bzip2
**命令用途**：`压缩或解压缩文件`

**压缩格式**：`.bz2`

**命令语法**：`bzip2[选项][文件]`

**常用选项**：

	-k：保留原文件；
	-d：解压文件；
	-v：显示执行过程；
<br/>
**用法示例**：

	//压缩并保留love.story 文件，生成love.story.bz2文件
	bzip2 -kv love.story 
	//解压缩love.story.bz2文件，并保留压缩文件
	bzip2 -dk love.story.bz2
常见的压缩格式及其所对应的Linux处理命令如下表： 

<table border="1" cellpadding="0" cellspacing="0" style="width:431px;"><tbody><tr><td style="text-align:center;width:127px;"><span style="color:#6600cc;"><strong>压缩格式</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#6600cc;"><strong>压缩命令</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#6600cc;"><strong>解压缩命令</strong></span></td></tr><tr><td style="text-align:center;width:127px;"><span style="color:#330033;"><strong>.gz</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#cc0000;"><strong>gzip</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#cc0000;"><strong>gunzip(等价gzip -d)</strong></span></td></tr><tr><td style="text-align:center;width:127px;"><span style="color:#330033;"><strong>.tar</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#cc0000;"><strong>tar -cf</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#cc0000;"><strong>tar -xf</strong></span></td></tr><tr><td style="text-align:center;width:127px;"><span style="color:#330033;"><strong>.tar.gz</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#cc0000;"><strong>tar -czf</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#cc0000;"><strong>tar -xzf</strong></span></td></tr><tr><td style="text-align:center;width:127px;"><span style="color:#330033;"><strong>.zip</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#cc0000;"><strong>zip</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#cc0000;"><strong>unzip</strong></span></td></tr><tr><td style="text-align:center;width:127px;"><span style="color:#330033;"><strong>.bz2</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#cc0000;"><strong>bzip2</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#cc0000;"><strong>bunzip2</strong></span></td></tr><tr><td style="text-align:center;width:127px;"><span style="color:#330033;"><strong>.tar.bz2</strong></span></td><td style="text-align:center;width:116px;"><span style="color:#cc0000;"><strong>tar -cjf</strong></span></td><td style="text-align:center;width:188px;"><span style="color:#cc0000;"><strong>tar -xjf</strong></span></td></tr></tbody></table>

# mount
**命令用途**：`加载文件系统到指定的挂载点`

**命令语法**：`mount[-t文件系统]设备文件名 挂载点`

**用法示例**：

	//挂载iso光盘镜像文件到mnt/cdrom目录，以下三种方式均可
	mount -t iso9660 /dev/sr0 /mnt/cdrom
	mount -t auto /dev/sr0 /mnt/cdrom
	mount /dev/sr0 /mnt/cdrom

# umount
**命令用途**：`卸载挂载的文件`

**命令语法**：`umount[设备文件名或挂载点]`

**用法示例**：

	//卸载挂载的iso光盘镜像，以下两种方式均可
	umount /mnt/cdrom
	umount /dev/sr0
 