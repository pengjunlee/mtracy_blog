# Scala简介
**Scala**（`Scalable Language`）是一门多范式（multi-paradigm）的编程语言，设计初衷是要集成面向对象编程和函数式编程的各种特性。

## Scala特性
### 面向对象
Scala是一种纯面向对象的语言。

### 函数式编程
Scala也是一种函数式语言，其函数也能当成值来使用。Scala提供了轻量级的语法用以定义匿名函数，支持高阶函数，允许嵌套多层函数，并支持柯里化。Scala的case class及其内置的模式匹配相当于函数式编程语言中常用的代数类型。更进一步，程序员可以利用Scala的模式匹配，编写类似正则表达式的代码处理XML数据。

### 兼容Java
Scala 的源代码可以被编译成Java字节码，运行于JVM之上，并可以调用现有的Java类库。
Scala语法简洁，代码行短。

### 静态类型
Scala具备类型系统，通过编译时检查，保证代码的安全性和一致性。

### 并发性
Scala使用Actor作为其并发模型，Actor是类似线程的实体，通过邮箱发收消息。Actor可以复用线程，因此可以在程序中可以使用数百万个Actor,而线程只能创建数千个。在2.10之后的版本中，使用Akka作为其默认Actor实现。

# Scala安装
## JDK环境
在安装Scala之前，请确保你本地已经安装了`JDK 1.5`以上版本。

使用以下命令检查是否安装了JDK：

	$ java -version
	java version "1.8.0_211"
	Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
	Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)

并使用以下命令检查是否安装了Java编译器：

	$ javac -version
	javac 1.8.0_211

## 下载Scala安装包
访问[Scala官网](Scala官网 "https://www.scala-lang.org/download/")，下载与你的操作系统版本对应的Scala二进制包，本例中的操作系统是Linux，所以我下载的版本是[scala-2.13.1.tgz](scala-2.13.1.tgz https://downloads.lightbend.com/scala/2.13.1/scala-2.13.1.tgz)。

## 解压安装包
将Scala安装包解压到你的安装目标目录，本例中是`/usr/local/`。

	$ tar -zxvf scala-2.13.1.tgz -C /usr/local/

## 添加环境变量

	vim /etc/profile

在`PATH`中加入`Scala`可执行脚本的路径。

	export PATH=$PATH:/usr/local/scala-2.13.1/bin

配置修改完成后:wq保存退出，并执行下面命令使配置生效。

	source /etc/profile

## 检查是否安装成功
执行`scala`命令，输出以下信息，表示安装成功：

	$ scala
	Welcome to Scala 2.13.1 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_211).
	Type in expressions for evaluation. Or try :help.
	 
	scala>

# Scala基础语法
由于本人是写Java出身，在此仅罗列部分Scala与Java在语法上的差异部分。

## 换行符
Scala语句末尾的分号`;`是可选的，Java末尾的分号必须有。

## Scala关键字
下表列出了scala保留关键字，我们不能使用以下关键字作为变量：
<table><tbody><tr><td> <p>abstract</p> </td><td> <p>case</p> </td><td> <p>catch</p> </td><td> <p>class</p> </td></tr><tr><td> <p>def</p> </td><td> <p>do</p> </td><td> <p>else</p> </td><td> <p>extends</p> </td></tr><tr><td> <p>false</p> </td><td> <p>final</p> </td><td> <p>finally</p> </td><td> <p>for</p> </td></tr><tr><td> <p>forSome</p> </td><td> <p>if</p> </td><td> <p>implicit</p> </td><td> <p>import</p> </td></tr><tr><td> <p>lazy</p> </td><td> <p>match</p> </td><td> <p>new</p> </td><td> <p>null</p> </td></tr><tr><td> <p>object</p> </td><td> <p>override</p> </td><td> <p>package</p> </td><td> <p>private</p> </td></tr><tr><td> <p>protected</p> </td><td> <p>return</p> </td><td> <p>sealed</p> </td><td> <p>super</p> </td></tr><tr><td> <p>this</p> </td><td> <p>throw</p> </td><td> <p>trait</p> </td><td> <p>try</p> </td></tr><tr><td> <p>true</p> </td><td> <p>type</p> </td><td> <p>val</p> </td><td> <p>var</p> </td></tr><tr><td> <p>while</p> </td><td> <p>with</p> </td><td> <p>yield</p> </td><td> <p>&nbsp;</p> </td></tr><tr><td> <p>-</p> </td><td> <p>:</p> </td><td> <p>=</p> </td><td> <p>=&gt;</p> </td></tr><tr><td> <p>&lt;-</p> </td><td> <p>&lt;:</p> </td><td> <p>&lt;%</p> </td><td> <p>&gt;:</p> </td></tr><tr><td> <p>#</p> </td><td> <p>@</p> </td><td>&nbsp;</td><td>&nbsp;</td></tr></tbody></table>
 
## Scala包
### 定义
Scala使用`package`关键字定义包，在Scala将代码定义到某个包中有两种方式：

第一种方法和 Java一样，在文件的头定义包名，这种方法就后续所有代码都放在该包中。 比如：

	package com.runoob
	class HelloWorld

第二种方法有些类似 C#，如：

	package com.runoob {
	  class HelloWorld 
	}

第二种方法，可以在一个文件中定义多个包。

### 包引用
Scala使用`import`关键字引用包。

	import java.awt.Color  // 引入Color
	 
	import java.awt._  // 引入包内所有成员
	 
	def handler(evt: event.ActionEvent) { // java.awt.event.ActionEvent
	  ...  // 因为引入了java.awt，所以可以省去前面的部分
	}

import语句可以出现在任何地方，而不是只能在文件顶部。import的效果从开始延伸到语句块的结束。这可以大幅减少名称冲突的可能性。

如果想要引入包中的几个成员，可以使用selector（选取器）：

	import java.awt.{Color, Font}
	 
	// 重命名成员
	import java.util.{HashMap => JavaHashMap}
	 
	// 隐藏成员
	import java.util.{HashMap => _, _} // 引入了util包的所有成员，但是HashMap被隐藏了

<font color=red>注意</font>：默认情况下，Scala总会引入`java.lang._`、`scala._`和`Predef._`，这里也能解释，为什么以scala开头的包，在使用时都是省去scala.的。

## Scala 数据类型
Scala与Java有着相同的数据类型，下表列出了Scala支持的数据类型：

<table><tbody><tr><th>数据类型</th><th>描述</th></tr><tr><td>Byte</td><td>8位有符号补码整数。数值区间为 -128 到 127</td></tr><tr><td>Short</td><td>16位有符号补码整数。数值区间为 -32768 到 32767</td></tr><tr><td>Int</td><td>32位有符号补码整数。数值区间为 -2147483648 到 2147483647</td></tr><tr><td>Long</td><td>64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807</td></tr><tr><td>Float</td><td>32 位, IEEE 754 标准的单精度浮点数</td></tr><tr><td>Double</td><td>64 位 IEEE 754 标准的双精度浮点数</td></tr><tr><td>Char</td><td>16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF</td></tr><tr><td>String</td><td>字符序列</td></tr><tr><td>Boolean</td><td>true或false</td></tr><tr><td>Unit</td><td>表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。</td></tr><tr><td>Null</td><td>null 或空引用</td></tr><tr><td>Nothing</td><td>Nothing类型在Scala的类层级的最底端；它是任何其他类型的子类型。</td></tr><tr><td>Any</td><td>Any是所有其他类的超类</td></tr><tr><td>AnyRef</td><td>AnyRef类是Scala里所有引用类(reference class)的基类</td></tr></tbody></table>

上表中列出的数据类型都是对象，也就是说scala没有java中的原生类型。在scala是可以对数字等基础类型调用方法的。

## Scala变量
在Scala中，使用关键词`var`声明变量，使用关键词`val`声明常量。

	var myVar : String = "Foo"
	val myVal : String = "Foo"

<font color=red>注意</font>：在Scala中声明变量和常量不一定要指明数据类型，在没有指明数据类型的情况下，其数据类型是通过变量或常量的初始值推断出来的。所以，如果在没有指明数据类型的情况下声明变量或常量必须要给出其初始值，否则将会报错。