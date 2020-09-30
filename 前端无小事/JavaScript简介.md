> 参考书籍：《JavaScript 权威指南----ECMAScript5+HTML5DOM+HTML5BOM》编著：张亚飞  

# JavaScript是什么？
`JavaScript`是一种基于对象(`Object`)和事件驱动(`EventDriven`)并具有安全性能的脚本语言。

要了解`JavaScript`，首先要了解什么是脚本语言。目前，动态的应用程序一般使用两种方式实现：`二进制方式`和`脚本方式`。

- 二进制(**Binary**)方式就是先将编写好的程序代码编译为机器可识别的指令代码，然后再执行。这种编译好的程序用户只能执行和使用，看不到原始程序的内容。
- 脚本(**Script**)方式是使用一种特殊的描述性语言，依据一定的格式编写的文本文件。简单地说，就是一条一条的文字命令，这些文字命令用户可以使用“记事本“程序看到。

脚本程序也是可执行文档，在执行时，有一个解释引擎(该解释引擎也是一个二进制的应用程序)将其逐条翻译成机器可识别的指令，并按程序顺序执行。

因为脚本在执行时多了一道翻译的程序，所以它比二进制程序的执行效率要稍低一些。

我们经常能看到的各种动态语言，如VBScript、JavaScript、JScript、PHP、CGI、JSP、CFML等，都是脚本语言。

## 客户端脚本

在脚本语言中，有些是作为客户端脚本语言来运行的，它们由客户端的解释引擎来解释。例如`VBScript`、`JavaScript`、`JScript`等都可以作为客户端脚本语言，当它们被嵌入到`HTML`文件中时，可以按照顺序执行或者响应某个事件从而对事件做出应答。客户端脚本语言一般用来创建动画效果、执行简单的验证等，从而丰富了网页的呈现。

## 服务端脚本

另外一些脚本语言是作为服务端脚本语言来运行的，例如`PHP`、`CGI`、`JSP`、`CFML`等，它们由服务端的解释引擎来解释。当作为服务端脚本来运行时，它们主要用来生产`HTML`内容，也可以动态生产客户端脚本。当被传到客户端的浏览器中时，这些客户端脚本代码也可以被解释并实现特定的功能。

其实有些脚本语言既可以用来编写客户端脚本代码，也可以用来编写服务端脚本代码，如：`JavaScript`等。但是目前将`JavaScript`作为服务器端代码的开发语言已经很少用了，并且也仅用于网景公司开发的应用程序服务器`Netscape Enterprise Server`中，目前该应用程序服务器也已经很少有人使用了。

> <font color=red>注</font>：有很多资料介绍可以使用`VBScript`和`JScript`开发`ASP`，但`ASP`是一个服务端技术，这看起来有些冲突，其实不然。`ASP`是一个技术统称，它可以使用`VBScript`和`JScript`来开发，因为使用`VBScript`和`JScript`既可以在服务端运行也可以在客户端运行，只要有其运行的环境(也就是解释器，或者称为解释引擎)即可，但要牢记，`VBScript`和`JScript`作为服务端脚本时，脚本是由服务端的解释引擎进行解释的，这与客户端脚本的运行环境有明显的不同，根本不是处在一个物理位置。 

# JavaScript和ECMAScript的关系
`JavaScript`最初由`Netscape`(网景公司)创建，名为`LiveScript`，后来才改名为`JavaScript`。1996年11月，`JavaScript`被`Netscape`公司提交给`ECMA`(**European Computer Manufactures Association**，即**欧洲计算机厂商协会**)，希望将其制订为标准。次年，`ECMA`发布262号标准文件（标准编号ECMA-262）的第一版，规定了浏览器脚本语言的编写规范，并将符合这一规范的语言称为`ECMAScript`，这个版本就是`ECMAScript 1.0`。直到2011年6月，`ECMAScript 5.1`被提交给**国际标准化组织**（`International Organization for Standardization`，简称`ISO`）和**国际电工委员会**(`International Electro technical Commission`,简称`IEC`)，ECMAScript才正式成为国际标准。

ECMAScript标准文件可访问以下地址进行查看：

<http://www.ecma-international.org/publications/standards/Ecma-262.htm>

`ECMAScript`标准从一开始其实是针对JavaScript语言制定的，但是之所以不叫`JavaScript`，主要有以下两个原因。

- JavaScript是Netscape公司的一个产品商标的名称，只有Netscape公司可以合法地使用JavaScript这个名字。
- 将标准名称定为ECMAScript，可以更好地体现这门语言的制定者是ECMA，而不是Netscape，这样做有利于保证该语言的开放性和中立性。

同时，`JavaScript`还是`ECMAScript`标准的一种实现（另外的实现了`ECMAScript`标准的语言还有`Jscript`和`ActionScript`）。

这里提到了IT行业中两个非常重要的名词----`标准`和`实现`。

- **标准**(`standard`)是由一个公认的机构制定和批准的文件。它对活动或活动的结果规定了规则、导则或特殊值，供共同和反复使用，以实现在预定领域内最佳秩序的效果。有一些标准具有强制力，例如国际标准化组织（**International Organization for Standardization**，简称`ISO`)制定的标准必须为其成员所遵守，且具有法定的约束力，另外一些标准则没有强制力，但具有很大的影响力，并且在很大程度上成为事实上的标准，如万维网联盟(World Wide Web Consortium,简称W3C)制定的一些标准，这些标准一般被称为规范(specification)，其中最著名的是HTTP协议，该协议实际上已经成为一种事实上的标准。
- **实现**(`implementation`)则是按照标准和规范做出的。例如，开发者按照HTTP协议开发出了一个浏览器程序，那么就称该浏览器程序为HTTP协议的一个实现，或者说该浏览器程序实现了HTTP协议。如IE，Firefox，网景等浏览器都是HTTP协议的实现。

`JScript`、`JavaScript`等都遵守`ECMA-262`标准，所以它们是`ECMA-262`标准的一个实现；解释引擎如果遵守`ECMA-262`标准，也可以称其为是`ECMA-262标准`的实现。如`SpiderMonkey`、`FlashPlayer`等都是`ECMA-262`标准的实现。 

# JavaScript和JScript的关系
`JavaScript`和`JScript`都是`ECMA-262`的实现，`JavaScript`是网景公司开发的一种脚本语言；`JScript`是微软公司开发的另一种脚本语言，是该公司对`ECMA-262`语言规范的一种实现。其实无论是`JavaScript`还是`JScript`都是部分遵守`ECMA-262`标准，并在该标准基础上扩展了自己某些特殊的功能。

`JavaScript`和`JScript`都既可以运行在客户端，也可以运行在服务端。但无论是运行在客户端还是服务端，其解释引擎都不相同。

在服务端，`JavaScript`由网景公司的服务端解释引擎(Netscape服务器Livewire)解释，并不属于ASP语法；而`JScript`与`VBScript`使用相同的服务端解释引擎解释，属于ASP语法。

在客户端，`JavaScript`和`JScript`的解释引擎种类繁多(最常见的就是浏览器)，几乎每个浏览器都支持`JavaScript`，但很少有浏览器支持`JScript`，除了微软的`IE`。 

# JavaScript和Java的关系
`Java`语言是著名的信息技术公司Sun发明的(目前Sun已经与Oracle公司合并)，用于在客户端和服务端运行的编程语言。它与`JavaScript`的关系就好比雷锋和雷峰塔的关系一样，除了名称相近，两者并无本质上的联系。

`JavaScript`和`Java`的区别主要体现在以下3个方面

- Java是一种严格的面向对象的程序设计语言，常用于开发基于Internet的应用程序。它是一种强类型语言，一旦一个变量被指定了某个数据类型，如果不经过强制转换，那么它就永远是这个数据类型了。
- JavaScript是一种脚本语言，常用于网页中增强交互性和页面效果，以及进行数据校验等。它是一种弱类型语言，数据类型可以被忽略的语言。它与强类型定义语言相反, 一个变量可以赋不同数据类型的值。
- JavaScript与Java的运行环境截然不同，使用Java语言开发的程序必须在JVM(Java虚拟机)内运行，而JavaScript一般在一个浏览器内或者其他的JavaScript解释引擎内运行。

两者名称之所以会相近，是由于在`Netscape`发展`LiveScript`的同时，`Sun`公司也正在发展`Java`语言，为了使双方都能受益，两家公司进行合作，`Netscape`将`LiveScript`语言改名为`JavaScript`，这就是`JavaScript`的由来。 

# JavaScript的开发和运行环境
工欲善其事,必先利其器，要使用`JavaScript`进行开发，就不可避免的要借助一些辅助工具：

- 开发环境----`Dreamweaver`、`Aptana Studio`、`Eclipse`等。
- 运行环境----`Internet Explorer`、`Firefox`、`Chrome`等浏览器。