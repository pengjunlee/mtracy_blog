> 原文链接：<https://www.cnblogs.com/ooo0/p/6290201.html>

[Sass](http://sass-lang.com/ "Sass")是成熟、稳定、强大的CSS预处理器，而`SCSS`是`Sass3`版本当中引入的新语法特性，完全兼容`CSS3`的同时继承了`Sass`强大的动态功能。

# 特性概览
`CSS`书写代码规模较大的Web应用时，容易造成选择器、层叠的复杂度过高，因此推荐通过`SASS`预处理器进行`CSS`的开发，`SASS`提供的变量、嵌套、混合、继承等特性，让`CSS`的书写更加有趣与程式化。

## 变量
变量用来存储需要在`CSS`中复用的信息，例如颜色和字体。`SASS`通过`$`符号去声明一个变量。

	$font-stack: Helvetica, sans-serif;
	$primary-color: #333;
	 
	body {
	  font: 100% $font-stack;
	  color: $primary-color;
	}
	
上面例子中变量`$font-stack`和`$primary-color`的值将会替换所有引用他们的位置。

	body {
	  font: 100% Helvetica, sans-serif;
	  color: #333; }

## 嵌套
`SASS`允许开发人员以嵌套的方式使用`CSS`，但是过度的使用嵌套会让产生的`CSS`难以维护，因此是一种不好的实践，下面的例子表达了一个典型的网站导航样式：

	nav {
	  ul {
	    margin: 0;
	    padding: 0;
	    list-style: none;
	  }
	 
	  li { display: inline-block; }
	 
	  a {
	    display: block;
	    padding: 6px 12px;
	    text-decoration: none;
	  }
	}

大家注意上面代码中的`ul`、`li`、`a`选择器都被嵌套在nav选择器当中使用，这是一种书写更高可读性`CSS`的良好方式，编译后产生的`CSS`代码如下：

	nav ul {
	  margin: 0;
	  padding: 0;
	  list-style: none; }
	nav li {
	  display: inline-block; }
	nav a {
	  display: block;
	  padding: 6px 12px;
	  text-decoration: none; }

## 引入
`SASS`能够将代码分割为多个片段，并以`underscore`风格的下划线作为其命名前缀（`_partial.scss`），`SASS`会通过这些下划线来辨别哪些文件是`SASS`片段，并且不让片段内容直接生成为`CSS`文件，从而只是在使用`@import`指令的位置被导入。`CSS`原生的`@import`会通过额外的`HTTP`请求获取引入的样式片段，而`SASS`的`@import`则会直接将这些引入的片段合并至当前`CSS`文件，并且不会产生新的`HTTP`请求。下面例子中的代码，将会在`base.scss`文件当中引入`_reset.scss`片断。

	// _reset.scss
	html, body, ul, ol {
	  margin:  0;
	  padding: 0;
	}
	 
	// base.scss
	@import 'reset';
	body {
	  font: 100% Helvetica, sans-serif;
	  background-color: #efefef;
	}

`SASS`中引入片断时，可以缺省使用文件扩展名，因此上面代码中直接通过`@import 'reset'`引入，编译后生成的代码如下：

	html, body, ul, ol {
	  margin: 0;
	  padding: 0; }
	 
	body {
	  font: 100% Helvetica, sans-serif;
	  background-color: #efefef; }

## 混合
混合（`Mixin`）用来分组那些需要在页面中复用的`CSS`声明，开发人员可以通过向`Mixin`传递变量参数来让代码更加灵活，该特性在添加浏览器兼容性前缀的时候非常有用，`SASS`目前使用`@mixin name`指令来进行混合操作。

	@mixin border-radius($radius) {
	          border-radius: $radius;
	      -ms-border-radius: $radius;
	     -moz-border-radius: $radius;
	  -webkit-border-radius: $radius;
	}
	 
	.box {
	  @include border-radius(10px);
	}

上面的代码建立了一个名为`border-radius`的`Mixin`，并传递了一个变量`$radius`作为参数，然后在后续代码中通过`@include border-radius(10px)`使用该`Mixin`，最终编译的结果如下：

	.box {
	  border-radius: 10px;
	  -ms-border-radius: 10px;
	  -moz-border-radius: 10px;
	  -webkit-border-radius: 10px; }

## 继承
继承是`SASS`中非常重要的一个特性，可以通过`@extend`指令在选择器之间复用CSS属性，并且不会产生冗余的代码，下面例子将会通过`SASS`提供的继承机制建立一系列样式：

	// 这段代码不会被输出到最终生成的CSS文件，因为它没有被任何代码所继承。
	%other-styles {
	  display: flex;
	  flex-wrap: wrap;
	}
	 
	// 下面代码会正常输出到生成的CSS文件，因为它被其接下来的代码所继承。
	%message-common {
	  border: 1px solid #ccc;
	  padding: 10px;
	  color: #333;
	}
	 
	.message {
	  @extend %message-common;
	}
	 
	.success {
	  @extend %message-common;
	  border-color: green;
	}
	 
	.error {
	  @extend %message-common;
	  border-color: red;
	}
	 
	.warning {
	  @extend %message-common;
	  border-color: yellow;
	}

上面代码将`.message`中的`CSS`属性应用到了`.success`、`.error`、`.warning`上面，魔法将会发生在最终生成的`CSS`当中。这种方式能够避免在`HTML`元素上书写多个`class`选择器，最终生成的`CSS`样式是下面这样的：

	.message, .success, .error, .warning {
	  border: 1px solid #ccc;
	  padding: 10px;
	  color: #333; }
	 
	.success {
	  border-color: green; }
	 
	.error {
	  border-color: red; }
	 
	.warning {
	  border-color: yellow; }

# 操作符
`SASS`提供了标准的算术运算符，例如`+`、`-`、`*`、`/`、`%`。在接下来的例子里，我们尝试在`aside`和`article`选择器当中对宽度进行简单的计算。

	.container { width: 100%; }
	 
	article[role="main"] {
	  float: left;
	  width: 600px / 960px * 100%;
	}
	 
	aside[role="complementary"] {
	  float: right;
	  width: 300px / 960px * 100%;
	}

上面代码以`960px`为基准建立了简单的流式网格布局，`SASS`提供的算术运算符让开发人员可以更容易的将像素值转换为百分比，最终生成的CSS样式如下所示：

	.container {
	  width: 100%; }
	 
	article[role="main"] {
	  float: left;
	  width: 62.5%; }
	 
	aside[role="complementary"] {
	  float: right;
	  width: 31.25%; }

# CSS扩展
## 引用父级选择器"&"
`Scss`使用`&`关键字在`CSS`规则中引用父级选择器，例如在嵌套使用伪类选择器的场景下：

	/*===== SCSS =====*/
	a {
	  font-weight: bold;
	  text-decoration: none;
	  &:hover { text-decoration: underline; }
	  body.firefox & { font-weight: normal; }
	}
	 
	/*===== CSS =====*/
	a {
	  font-weight: bold;
	  text-decoration: none; }
	  a:hover {
	    text-decoration: underline; }
	  body.firefox a {
	    font-weight: normal; }

无论`CSS`规则嵌套的深度怎样，关键字`&`都会使用父级选择器级联替换全部其出现的位置：

	/*===== SCSS =====*/
	#main {
	  color: black;
	  a {
	    font-weight: bold;
	    &:hover { color: red; }
	  }
	}
	 
	/*===== CSS =====*/
	#main {
	  color: black; }
	  #main a {
	    font-weight: bold; }
	    #main a:hover {
	      color: red; }

`&`必须出现在复合选择器开头的位置，后面再连接自定义的后缀，例如：

	/*===== SCSS =====*/
	#main {
	  color: black;
	  &-sidebar { border: 1px solid; }
	}
	 
	/*===== CSS =====*/
	#main {
	  color: black; }
	  #main-sidebar {
	    border: 1px solid; }

如果在父级选择器不存在的场景使用`&`，`Scss`预处理器会报出错误信息。

## 嵌套属性
`CSS`许多属性都位于相同的命名空间（例如`font-family`、`font-size`、`font-weight`都位于`font`命名空间下），`Scss`当中只需要编写命名空间一次，后续嵌套的子属性都将会位于该命名空间之下，请看下面的代码：

	/*===== SCSS =====*/
	.demo {
	  // 命令空间后带有冒号:
	  font: {
	    family: fantasy;
	    size: 30em;
	    weight: bold;
	  }
	}
	 
	/*===== CSS =====*/
	.demo {
	  font-family: fantasy;
	  font-size: 30em;
	  font-weight: bold; }

命令空间上可以直接书写`CSS`简写属性，但是日常开发中建议直接书写`CSS`简写属性，尽量保持`CSS`语法的一致性。

	.demo {
	  font: 20px/24px fantasy {
	    weight: bold;
	  }
	}
	 
	.demo {
	  font: 20px/24px fantasy;
	    font-weight: bold;
	}

> 注释：转自 <https://uinika.github.io/web/scss.html>