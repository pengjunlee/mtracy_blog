# find
**命令用途**：`在指定目录下搜索文件或目录`

**命令语法**：`find [搜索范围] [-选项] [匹配条件]`

**常用选项**：

	-name：根据文件名(区分大小写)进行搜索；
	-iname：根据文件名(不区分大小写)进行搜索；
	-size：根据文件的大小（以数据块为单位，1数据块=0.5K=512字节）进行搜索；
	-user：根据文件的所有者进行搜索；
	-group：根据文件的所属组进行搜索；
	-amin：根据文件的访问时间进行搜索；
	-cmin：根据文件属性的修改时间进行搜索；
	-mmin：根据文件内容的修改时间进行搜索；
	-type：根据文件类型进行搜索（f文件、d目录、l软链接文件）；
	-inum：根据i节点进行搜索；
	-exec/-ok（询问确认）：对搜索结果执行操作；
<br/>
**用法示例**：

	// 搜索/etc目录下文件名为init的文件或目录
	find /etc -name init
	// 搜索/etc目录下文件名中包含init的文件或目录
	find /etc -name *init*
	// 搜索/etc目录下文件名以init开头，后面跟3个其他字符的文件或目录
	find /etc -name init???
	
	// 搜索/etc目录下文件大小大于100MB的文件
	find /etc -size +204800
	// 搜索/etc目录下文件大小大于50MB且小于100MB的文件
	find /etc -size +102400 -a -size -204800
	// 搜索/etc目录下文件大小等于100MB或者文件名以init开头的文件
	find /etc -size 204800 -o -name init*

	// 搜索/etc目录下文件的所有者为root的文件或目录
	find /etc -user root
	// 搜索/etc目录下文件的所属组为root的文件或目录
	find /etc -group root
 
	//搜索/etc目录下5分钟之内被访问过的文件或目录
	find /etc -amin -5
	//搜索/etc目录下5分钟之内属性被修改过的文件或目录
	find /etc -cmin -5
	//搜索/etc目录下5分钟之内内容被修改过的文件或目录
	find /etc -mmin -5
	
	//搜索/etc目录下的软链接文件
	find /etc -type l
	//搜索/etc目录下文件名以init开头的目录
	find /etc -name init* -a -type d
	//搜索/etc目录下i节点号为917211的文件
	find /etc -inum 917211
	
	//搜索/etc目录下文件名以init开头的文件或目录，并显示其详细信息
	find /etc -name init* -exec ls -l {} \;
	//搜索/etc目录下文件名以init开头的文件，并显示其详细信息
	find /etc -name init* -a -type f -ok ls -i {} \;

# locate
**命令用途**：`locate`命令其实是`find -name`的另一种写法，但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库`/var/lib/locatedb`，这个数据库中含有本地所有文件信息。Linux系统自动创建这个数据库，并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。为了避免这种情况，可以在使用`locate`之前，先使用`updatedb`命令，手动更新数据库。

**命令语法**：`locate[-选项][文件名]`

**常用选项**：

	-i：不区分大小写； 
<br/>
**用法示例**：

	//使用locate命令搜索之前，先使用updatedb命令，手动更新数据库
	updatedb
	// 搜索etc目录下文件路径中包含init（不区分大小写）的文件或目录
	locate -i etc/INIT

# which
**命令用途**：`在环境变量$PATH设置的目录里查找并显示命令的绝对路径`

**命令语法**：`which[命令名]`

**用法示例**：
	
	// 查看ls命令的绝对路径
	which ls

# whereis
**命令用途**：`查询指定命令的二进制文件、源代码文件和man帮助手册文件等相关文件的路径`

**命令语法**：`whereis[-选项][命令名]`

**常用选项**：

	-b：只查找二进制文件；
	-m：只查找帮助文件；
	-s：只查找源代码文件； 
<br/>
**用法示例**：

	// 搜索ls命令的二进制文件的绝对路径
	whereis -b ls
	// 搜索ls命令的帮助手册的绝对路径
	whereis -m ls

# grep
**命令用途**：`grep`命令是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

**命令语法**：`grep[-选项][匹配条件][文件名]`

**常用选项**：
	
	-i：不区分大小写；
	-v：排除指定字串； 
<br/>
**用法示例**：

	// 搜索etc目录下的inittab文件中包含multiuser （不区分大小写）的行
	grep -i multiuser /etc/inittab
	// 搜索etc目录下的inittab文件中不以“#”开头的行
	grep -v ^# /etc/inittab
	// 搜索etc目录下的inittab文件中不以“,”结尾的行
	grep -v ,$ /etc/inittab