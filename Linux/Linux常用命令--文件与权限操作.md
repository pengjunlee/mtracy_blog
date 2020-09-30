# ls
**命令用途**：`显示目录或文件列表`

**命令语法**：`ls [-选项] [文件或目录]`

**常用选项**：

	-a：显示所有文件及目录（包括“.”、“..”以及其它以“.”开头的隐藏文件）；
	-A：显示除隐藏文件“.”和“..”以外的所有文件及目录； 
	-l：以长格式显示目录下的文件或目录。输出的信息从左到右依次为：文件类型（“-”二进制文件、“d”目录 、“l”软链接文件）+操作权限（用9个字符来分别表示所有者、所属组、其他人对文件的可操作权限：“r”读 、“w”写 、“x”执行）、硬连接引用计数、所有者、所属组、文件大小、文件最后一次的修改时间和文件名称；
	-i：显示文件索引节点号（inode），一个索引节点代表一个文件；
	-h：更加人性化地展示文件的大小，单位是G，M，K，Byte。
	-d：显示指定目录本身的信息；
<br/>
**用法示例**：

	// 显示当前目录下的所有文件
	ls -a
	// 显示root目录下除隐藏文件“.”和“..”以外的所有文件
	ls -A /root/
	//人性化地显示root目录本身的节点号及详细信息
	ls -ldhi /root/
	//人性化地显示root目录下anaconda-ks.cfg和install.log两个文件的节点号及详细信息
	ls -lhi /root/anaconda-ks.cfg /root/install.log
 
# mkdir
**命令用途**：`创建新目录`

**命令语法**：`mkdir [-选项] [目录名]`

**常用选项**：

	-p或--parent：递归创建；
	-m或--mode：创建目录的同时设置目录的操作权限；  
<br/>
**用法示例**：

	//在tmp目录下同时创建dubbo和conf两个目录
	mkdir /tmp/dubbo /tmp/conf
	//在tmp目录下递归创建solr/data目录，并设置其操作权限为750
	mkdir -pm 750 /tmp/solr/data

# touch
**命令用途**：`创建空文件或更新文件的时间属性`

**命令语法**：`touch[文件名]`

**用法示例**：

	//在tmp目录下创建一个名称为love.story的空文件，若文件已存在则更新文件的时间属性
	touch /tmp/love.story

# cd
**命令用途**：`切换工作目录`

**命令语法**：`cd [目录]`

**用法示例**：

	//切换到用户pengjunlee的家目录
	cd /home/pengjunlee/
	//切换到当前目录的上一级目录下的tmp目录
	cd ./../tmp

> 注：在Linux中，`.`表示当前目录、`..`表示当前目录的上一级目录  

# pwd
**命令用途**：`显示当前工作目录的绝对路径`

**命令语法**：`pwd`

**用法示例**：

	//显示当前工作目录的绝对路径
	pwd

# rmdir
**命令用途**：`删除空目录`

**命令语法**：`rmdir [目录]`

**用法示例**：

	//删除tmp目录下的conf目录
	rmdir /tmp/conf/

# cp
**命令用途**：`复制文件或目录`

**命令语法**：`cp [-选项] [原文件或目录] [目标目录]`

**常用选项**：

	-p：复制的同时，保留文件的属性
	-r：复制目录 
<br/>
**用法示例**：

	//将root家目录的install.log和install.log.syslog两个文件拷贝到/tmp目录下，并保留文件的属性
	cp -p /root/install.log /root/install.log.syslog /tmp/
	//将tmp目录下的mysql目录复制到test目录下，并将目录名改为mysql5.6
	cp -r /tmp/mysql/ /test/mysql5.6
	//将tmp/mysql目录下的mysql.conf文件复制到test目录下，并将其文件名改为default.conf
	cp /tmp/mysql/mysql.conf /test/default.conf

# mv
**命令用途**：`移动文件或目录`

**命令语法**：`mv [-选项] [原文件或目录] [目标目录]`

**常用选项**：

	-b：当文件存在时，覆盖前，为其创建一个备份； 
	-f：若目标文件或目录与现有的文件或目录重复，则直接覆盖现有的文件或目录； 
	-i：交互式操作，覆盖前先行询问用户，如果源文件与目标文件或目标目录中的文件同名，则询问用户是否覆盖目标文件。用户输入”y”，表示将覆盖目标文件；输入”n”，表示取消对源文件的移动。这样可以避免误将文件覆盖。
	-u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作。 
<br/>
**用法示例**：

	//将tmp目录下的install.log和install.log.syslog两个文件拷贝到/test目录下
	mv /tmp/install.log /tmp/install.log.syslog /test/
	//将tmp目录下的mysql目录移动到test目录下，并将目录名改为mysql5.6
	mv /tmp/mysql /test/mysql5.6
	//将tmp目录下的readme.txt文件移动到test目录下，若文件已存在，则在覆盖前为其创建一个备份
	mv -b /tmp/readme.txt /test/

# rm
**命令用途**：`删除文件或目录`

**命令语法**：`rm [-选项] [文件或目录]`

**常用选项**：

	-r：删除目录
	-f：强制执行 
<br/>
**用法示例**：

	//强制删除test目录下的install.log和install.log.syslog两个文件
	rm -f /test/install.log /test/install.log.syslog 
	//强制删除test目录下的mysql5.6目录
	rm -rf /test/mysql5.6/

# ln
**命令用途**：`为文件创建链接`

**命令语法**：`ln[-选项][源文件][目标文件]`

**常用选项**：

	-s：创建软链接； 
<br/>
**用法示例**：

	// 为mysql.conf文件创建软链接mysql.soft
	ln -s mysql.conf mysql.soft
	// 为mysql.conf文件创建硬链接mysql.hard
	ln mysql.conf mysql.hard
 

> 注：
> 1. 软链接类似于Windows中的快捷方式，默认操作权限为->`rwxrwxrwx`。
> 2. 硬链接相当于保留属性复制+同步更新，源文件与硬链接文件的i节点号相同。
> 3. 硬链接不能跨分区，且不能为目录创建硬链接。

# chmod
**命令用途**：`变更文件或目录的权限`

**命令语法**：`chmode[{ugoa}{+-=}{rwx}]|[mode=421 ][-选项] [文件或目录]`

**常用选项**：

	-R：递归修改； 
<br/>
**用法示例**：

	// 为mysqld.sh文件的所有者增加执行权限
	chmod u+x mysqld.sh
	// 取消mysqld.sh文件的所属组对文件的读权限
	chmod g-r mysqld.sh
	// 修改mysqld.sh文件的操作权限为：所有者具有读写执行权限，所属组具有读写权限，其他人具有读权限
	chmod u=rw,g=rw,o=r mysqld.sh 
	// 修改所有人对mysqld.sh文件的操作权限为读写权限
	chmod a=rw mysqld.sh 
	// 修改mysql目录及其下所有文件的操作权限，各角色均具有读写权限
	chmod 666 -R mysql/

> 注：读、写、执行权限针对文件和目录分别有着不同的含义，如下表所示。 

<table border="1" cellpadding="0" cellspacing="0" style="width:541px;"><tbody><tr><td style="text-align:center;width:70px;">代表字符</td><td style="text-align:center;width:77px;">权限</td><td style="text-align:center;width:127px;">对文件的含义</td><td style="text-align:center;width:267px;">对目录的含义</td></tr><tr><td style="text-align:center;width:70px;">r</td><td style="text-align:center;width:77px;">读权限</td><td style="text-align:center;width:127px;">可以查看文件内容</td><td style="text-align:center;width:267px;">可以列出目录中的内容</td></tr><tr><td style="text-align:center;width:70px;">w</td><td style="text-align:center;width:77px;">写权限</td><td style="text-align:center;width:127px;">可以修改文件内容</td><td style="text-align:center;width:267px;">可以在目录中创建、删除文件</td></tr><tr><td style="text-align:center;width:70px;">x</td><td style="text-align:center;width:77px;">执行权限</td><td style="text-align:center;width:127px;">可以执行文件</td><td style="text-align:center;width:267px;">可以进入目录</td></tr></tbody></table>

# chown
**命令用途**：`变更某个文件或目录的所有者`

**命令语法**：`chown[所有者] [文件或目录]`

**常用选项**：

	-R：递归修改； 
<br/>
**用法示例**：

	// 将mysqld.sh文件的所有者变更为pengjunlee
	chown pengjunlee mysqld.sh
	// 将mysql目录及其下所有文件的所有者变更为pengjunlee
	chown pengjunlee -R mysql/

# chgrp
**命令用途**：`变更某个文件或目录的所属组`

**命令语法**：`chgrp[所属组] [文件或目录]`

**常用选项**：

	-R：递归修改； 
<br/>
**用法示例**：

	// 将mysqld.sh文件的所属组变更为guest 
	chgrp guest mysqld.sh 
	// 将mysql目录及其下所有文件的所属组变更为guest 
	chgrp guest -R mysql/

# umask
**命令用途**：`显示、设置新建文件的缺省权限`

**命令语法**：`umask[选项] [文件或目录]`

**常用选项**：

	-S：以rwx形式显示新建文件的缺省权限； 
<br/>
**用法示例**：

	// 以rwx形式显示新建文件的缺省权限
	umask -S
	// 设置新建文件的缺省权限为077，即所有者具有读写和执行权限，其他角色无任何权限
	umask 077

> 注：
> 1. umask默认以求反的方式来显示缺省权限，例如0022，则默认权限为`755`=（`777`-`022`）。
> 2. 新建文件会自动将缺省权限中的执行权限去除，即各角色对新建文件都将不具有执行权限。