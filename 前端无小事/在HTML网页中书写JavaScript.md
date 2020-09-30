> 参考书籍：《JavaScript 权威指南----ECMAScript5+HTML5DOM+HTML5BOM》编著：张亚飞

# 使用script元素定义JavaScript脚本代码
在HTML文档中，可以使用使用`<script>`元素向HTML页面中插入`JavaScript`脚本，`<script>`元素的语法格式如下： 

	<script type="脚本语言类型" src="外部脚本文件的URL"
	             defer="defer" async="async"
	             charset="外部脚本文件的字符编码">
	 //<!--
	 JavaScript代码
	 //-->
	</script>

<font color=blue>参数说明</font>： 

- language="脚本语言类型" 

已废弃。原来用于指定`<script>`元素的内容所使用的脚本语言，现已使用`type`属性来代替。

- type="脚本语言类型" 

必需。该属性可以看成是language 的替代属性，用于指定`<script>`元素的内容所使用的脚本语言，属性值是`W3C`标准定义的内容类型（也称为MIME类型），例如：`text/tcl`、`text/javascript`和`text/vbscript`。该属性在HTML4时没有默认值，HTML5规定其默认值是`text/javascript`。

- src="外部脚本文件的URL" 

可选。该属性指定了所引用的外部脚本文件的URL。

- defer="defer" 

可选。该属性用来通知浏览器，这段脚本代码不会产生任何文档内容，可以延迟到文档完全被解析和显示之后再执行。例如 `javascript`脚本中的`document.write()`方法可以用来在当前的文档中创建内容，但也不是所有的`javascript`脚本都会改变文档的内容。如果您的脚本不会改变文档的内容，可将`defer`属性加入到`<script>`标签中，以便加快处理文档的速度。因为浏览器知道它将能够安全地读取文档的剩余部分而不用执行脚本，它将推迟对脚本的解释，直到文档已经显示给用户为止。

- async="async" 

可选。该属性用来指示是否异步加载所引用的外部脚本文件(仅适用于引入外部脚本)。

在HTML中，使用`<script>`元素插入`JavaScript`脚本代码时，必需将代码放在`<script type="text/javascript">`和`</script>`标记对之间，有以下两种方式：

在`<script>`元素中嵌入脚本代码 

	<script>
	//<!--
	这里书写JavaScript代码
	//-->
	</script>

例如下面的HTML代码片段： 

	<script>
	//<!--
	function popupMsg(msg){
	alert(msg);
	}
	//-->
	</script>

通过`<script>`元素中的`src`属性引入外部脚本文件 

	<script src="这里书写JavaScript脚本文件的文件路径">
	</script>

例如在HTML页面中引入JQuery函数库文件： 

	<script type="text/JavaScript" src="scripts/jquery-1.5.2.min.js"></script>

使用`<script>`元素定义的`JavaScript`脚本代码可以出现在HTML页面的`<head></head>`和`<body></body>`元素中，脚本代码写在这两个位置的区别如下：

- 在<head></head>元素中的脚本代码会在被调用的时候才执行。
- 在<body></body>元素中的脚本代码会在页面加载的时候被执行。

当然，你还可以把`<script>`元素定义的`JavaScript`脚本代码写到`<html></html>`元素的外面，不过这样的用法一般并不常见。 

# 在事件属性中定义JavaScript脚本代码
`JavaScript`脚本代码也可以定义在事件属性值中，当用户和浏览器交互时，只要事件被触发，相应的脚本就会被执行。

例如下面这段HTML脚本代码： 

	<div onclick="alert('这个div被点击了！');">请点击这里！</div>

# 超链接中定义JavaScript脚本代码
用户还可以为超链接元素`<a></a>`的`href`属性定义`JavaScript`脚本代码，这就相当于是在浏览器的地址栏中执行`JavaScript`脚本代码。

例如下面的代码，前一个链接使用了一个事件属性配合，而第二个链接没有使用事件属性，二者可以实现相同的功能： 

	<script>
	//<!--
	function popupMsg(msg){
	alert(msg);
	}
	//-->
	</script>
	<a href="javascript:void(0);" onclick="popupMsg('这是一个空链接')">这是一个空链接</a>
	<a href="javascript:popupMsg('这是一个空链接');" onclick="">这是一个空链接</a>

当单击该链接时，都会调用一个自定义的函数`popupMsg()`，该函数会弹出一个警告框。

该方法还可以用来实现打开新窗口这样的操作，例如下面的代码将会打开一个新的空窗口： 

	<a href="javascript:window.open('about:blank');">打开新窗口</a>

甚至于，用户可以在浏览器地址栏中输入一段以`javascript:`作为前缀的脚本代码，都会被执行，例如下面的代码： 

	javascript:window.open('about:blank');

对于一些浏览器，如果在浏览器地址栏中输入以上语句，将会打开一个新窗口或新标签页，如图所示。 
<div align=center>

![javascript](./imgs/38.png "javascript示意图")
<div align=left>

# 浏览器不支持脚本时应注意的问题
在HTML4中，规定可以在`<script>`元素中使用注释包含`JavaScript`代码，例如： 

	<script>
	//<!-- JavaScript代码
	function square(i){
	return i*i;
	}
	document.write("结果是： ",square(5));
	//-->
	</script>

或者： 

	<script>
	<!-- JavaScript代码
	function square(i){
	return i*i;
	}
	document.write("结果是： ",square(5));
	-->
	</script>

HTML4之所以这样规定，是因为当时还有很多浏览器不支持`JavaScript`，甚至也不支持`<script>`元素，当浏览器不支持`<script>`元素时，这样做就可以避免这些代码直接呈现为文本。

但是，当前不会再存在这种情况，`<script>`元素是`HTML5`标准所明确规定的，凡支持`HTML5`的浏览器都会支持`<script>`元素。

另外，还可以在HTML网页中在定义`<script>`元素的同时定义`<noscript>`元素，如果客户端浏览器可以识别脚本但设置了禁止执行脚本，那么就会执行`<noscript>`元素中的内容。例如： 

	<noscript>
	<p>浏览器不支持JavaScript...</p>
	</noscript>

# 声明脚本语言
因为HTML并不依赖于某个特定的脚本语言，所以文档创作者必须显式地告诉浏览器每个脚本所使用的语言，可以通过一个默认声明或者本地声明来完成。

## 默认的脚本语言

一般，应该在文档头部定义所使用脚本的默认语言，可以通过`<meta>`元素来实现。指定默认语言的`<meta>`元素声明的格式如下： 

	<meta http-equiv="Content-Script-Type" content="脚本语言类型"></meta>

这里，`content`属性的值是W3C标准定义的内容类型，比较常用的就是:"text/tcl"，"text/javascript"和"text/vbscript"。

如果没有使用`<meta>`元素指定默认语言，还可以通过HTTP报头的`Content-Script-Type`来设置默认语言。如下所示：

	Content-Script-Type：脚本语言类型

## 脚本语言的本地声明

通过`<script>`元素的`type`属性可以指定其中的脚本代码所使用的脚本语言，这被称为本地声明。本地声明的脚本语言将会覆盖文档默认的脚本语言。 