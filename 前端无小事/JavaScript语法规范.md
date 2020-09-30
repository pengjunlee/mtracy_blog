> 参考书籍：《JavaScript 权威指南----ECMAScript5+HTML5DOM+HTML5BOM》编著：张亚飞  

`JavaScript`编写语法遵循`ECMAScript`标准，以下是`ECMAScript`语言的一些基本规范。  

# 标识符的命名规范
`ECMAScript`标识符遵循以下标准命名规则：

- 第一字符必须是为字母、下划线( _ )或者美元符号( $ )。
- 其他字符可以是字母、下划线、美元符号或数字，最好不要包含其他字符。
- 不能把关键字或者保留字作为标识符。

例如下面的代码都是错误的： 

	var 5count=0; 			//首字符不能使用数字
	var yes/no=false;		//包含非法字符“/”
	var undefined="undefined";	//undefined是内建常量关键字

# 程序注释
## 单行注释和尾随注释

使用双斜线`//`可以定义单行注释或尾随注释。

例如下面的代码： 

	var oDate=new Date(); 	// 创建新的日期对象
	// 检查今天是否是星期日
	if(day=="sun"){
	}
## 多行注释

多行注释又被称为块注释，可以使用`/*`和`*/`进行定义，位于注释开始标签`/*`和注释结束标签`*/`之间的任何字符都将被解释为注释并忽略。

例如下面的代码： 

	/* 本例采用多行的注释方式
	*/

## 文档注释

文档注释以`/**`开始，以`*/`结束，且每行都以一个星号`*`开头。

例如下面的代码： 

	/**
	  * 该类为文档注释示例类
	  * @author pengjunlee
	  * @versioin 1.0.0.1
	  * @since js 1.5
	  */	 
	  function HelloWorld(){}

## HTML注释

HTML注释以`<!--`开始，以`-->`结束，例如下面的代码： 

	<!--这里是HTML注释-->

# 常用标识符命名方法
## 驼峰命名法（Camel Notation）

第一个单词首字母小写，其余所有单词首字母大写。变量、函数、方法、属性等基本都采用这种命名方法，例如下面的定义： 

	function displayUserInfo(){};
	var userName;

## 帕斯卡名法（Pascal Notation）

所有单词首字母大写。经常被用在类、接口的声明中，例如，`HelloWorld`就可以作为一个类名，而接口名经常在前面加一个大写字母`I`，例如`IHelloWorld`。

## 匈牙利命名法（Hungarian Notation）

在标识符前面增加小写字母做前缀，多用于C、C++的标识符命名。其基本规则是：

	标识符名称=特性前缀+功能描述

例如变量`m_wndStatusBar`，前缀`m_`表示类的成员，`wnd`也是前缀，表示的是变量对象特性，这里`wnd`的意义是窗口，所以`m_wnd`表示窗口类的成员，而`StatusBar`则是变量的功能描述。

以下是`JavaScript`常用到的匈牙利命名法前缀： 

<table border="1" cellpadding="0" cellspacing="0" style="width:477px;"><tbody><tr><td style="vertical-align:top;width:121px;">类型</td>
	<td style="vertical-align:top;width:83px;">前缀</td>
	<td style="vertical-align:top;width:113px;">类型</td>
	<td style="vertical-align:top;width:160px;">实例</td>
</tr><tr><td style="vertical-align:top;width:121px;">数组</td>
	<td style="vertical-align:top;width:83px;">a</td>
	<td style="vertical-align:top;width:113px;">Array</td>
	<td style="vertical-align:top;width:160px;">aItems</td>
</tr><tr><td style="vertical-align:top;width:121px;">布尔值</td>
	<td style="vertical-align:top;width:83px;">b</td>
	<td style="vertical-align:top;width:113px;">Boolean</td>
	<td style="vertical-align:top;width:160px;">bIsComplete</td>
</tr><tr><td style="vertical-align:top;width:121px;">浮点数</td>
	<td style="vertical-align:top;width:83px;">f</td>
	<td style="vertical-align:top;width:113px;">Float</td>
	<td style="vertical-align:top;width:160px;">fPrice</td>
</tr><tr><td style="vertical-align:top;width:121px;">整数</td>
	<td style="vertical-align:top;width:83px;">i</td>
	<td style="vertical-align:top;width:113px;">Integer</td>
	<td style="vertical-align:top;width:160px;">iItemCount</td>
</tr><tr><td style="vertical-align:top;width:121px;">对象</td>
	<td style="vertical-align:top;width:83px;">o</td>
	<td style="vertical-align:top;width:113px;">Object</td>
	<td style="vertical-align:top;width:160px;">oDiv1</td>
</tr><tr><td style="vertical-align:top;width:121px;">正则表达式</td>
	<td style="vertical-align:top;width:83px;">reg</td>
	<td style="vertical-align:top;width:113px;">RegExp</td>
	<td style="vertical-align:top;width:160px;">reEmailCheck</td>
</tr><tr><td style="vertical-align:top;width:121px;">字符串</td>
	<td style="vertical-align:top;width:83px;">s</td>
	<td style="vertical-align:top;width:113px;">String</td>
	<td style="vertical-align:top;width:160px;">sUserName</td>
</tr><tr><td style="vertical-align:top;width:121px;">变体变量</td>
	<td style="vertical-align:top;width:83px;">v</td>
	<td style="vertical-align:top;width:113px;">Variant</td>
	<td style="vertical-align:top;width:160px;">vAnything</td>
</tr><tr><td style="vertical-align:top;width:121px;">函数</td>
	<td style="vertical-align:top;width:83px;">fn</td>
	<td style="vertical-align:top;width:113px;">Function</td>
	<td style="vertical-align:top;width:160px;">fnHandler</td>
</tr></tbody></table>

# ECMAScript5严格模式
严格模式（`Strict Mode`）是`ECMAScript5`新增的功能，使用严格模式可以捕捉到一些常见的代码错误，抛出异常。当一些相对来说不安全的操作执行时，使用严格模式可以阻止或者抛出异常。

要在全局范围内使用严格模式，只需在程序第一行定义下面的一行代码： 

	"use strict";

要在函数内使用严格模式，只需在函数体内第一行定义下面的一行代码：  

	function fnInStrictMode(){
	      "use strict";
	      //... 其他代码 ...
	}

以为严格模式仅仅是使用一行文本字符串声明来实现，所以对于旧的不支持严格模式的浏览器来说不存在兼容性问题，因此可以放心大胆地使用。