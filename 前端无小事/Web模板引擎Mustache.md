> 原文地址：<http://www.iinterest.net/2012/09/12/web-template-engine-mustache/>

Web模板引擎是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，通常是标准的 HTML 文档。当然不同的开发语言有不同模板引擎，如`Javascript`下的`Hogan`、`ASP`下的`aspTemplate`、以及`PHP`下的 `Smarty`，这里主要介绍基于`Javascript`语言的模板引擎，目前流行的有`Mustache`、`Hogan`、`doT.js`、`JsRender`、`Kendo UI Templates`等，[jsperf.com](http://jsperf.com/dom-vs-innerhtml-based-templating/414 "jsperf.com") 上可以看到它们的性能对比，首先先介绍下[Mustache](http://mustache.github.io/ "Mustache")。  

# Mustache简介
`Mustache`是一个`logic-less`（轻逻辑）模板解析引擎，它的优势在于可以应用在`Javascript`、`PHP`、`Python`、`Perl`等多种编程语言中。

# Mustache语法
`Mustache`的模板语法很简单，就那么几个：

	{{keyName}}
	{{#keyName}} {{/keyName}}
	{{^keyName}} {{/keyName}}
	{{.}}
	{{<partials}}
	{{{keyName}}}
	{{!comments}}

这里将以`javascript`应用为例进行介绍，先来看个Demo： 

	<script type="text/javascript" src="mustache.js"></script>
	<script type="text/javascript">
	var data = {
	    "company": "Apple",
	    "address": {
	        "street": "1 Infinite Loop Cupertino</br>",
	        "city": "California ",
	        "state": "CA ",
	        "zip": "95014 "
	    },
	    "product": ["Macbook ","iPhone ","iPod ","iPad "]
	}
	var tpl = '<h1>Hello {{company}}</h1>';
	var html = Mustache.render(tpl, data);
	console.log( html )
	</script>
	...
	//运行后 Console 输出：
	<h1>Hello Apple</h1>

在Demo中可以看到`data`是数据，`tpl`是定义的模板，`Mustache.render(tpl, data)`方法是用于渲染输出最终的 `HTML`代码。

借助Demo来对语法做简单的介绍。

## {{keyName}}

`{{}}`就是`Mustache`的标示符，花括号里的`keyName`表示键名，这句的作用是直接输出与键名匹配的键值，例如： 

	var tpl = '{{company}}';
	var html = Mustache.render(tpl, data);
	//输出：
	Apple
	
## {{#keyName}} {{/keyName}}

以`#`开始、以`/`结束表示区块，它会根据当前上下文中的键值来对区块进行一次或多次渲染，例如改写下Demo中的 tpl：

	var tpl = '{{#address}} <p>{{street}},{{city}},{{state}}</p> {{/address}}';
	var html = Mustache.render(tpl, data);
	 
	//输出：
	<p>1 Infinite Loop Cupertino</br>,California,CA</p>

<font color=red>注意</font>：如果`{{#keyName}} {{/keyName}}`中的`keyName`值为`null`, `undefined`, `false`，则不渲染输出任何内容。 

## {{^keyName}} {{/keyName}}

该语法与`{{#keyName}} {{/keyName}}`类似，不同在于它是当`keyName`值为`null`, `undefined`, `false`时才渲染输出该区块内容。 

	var tpl = '{{^nothing}}没找到 nothing 键名就会渲染这段{{/nothing}}';
	var html = Mustache.render(tpl, data);
	//输出：
	没找到 nothing 键名就会渲染这段

## {{.}}
`{{.}}`表示枚举，可以循环输出整个数组，例如：

	var tpl = '{{#product}} <p>{{.}}</p> {{/product}}';
	var html = Mustache.render(tpl, data);
	//输出：
	<p>Macbook </p> <p>iPhone </p> <p>iPod </p> <p>iPad </p>

## {{>partials}}
以`>`开始表示子模块，如`{{> address}}`；当结构比较复杂时，我们可以使用该语法将复杂的结构拆分成几个小的子模块，例如：

	var tpl = '<h1>{{company}}</h1> <ul>{{>address}}</ul>';
	var partials = {address: "{{#address}}<li>{{street}}</li><li>{{city}}</li><li>{{state}}</li><li>{{zip}}</li>{{/address}}"}
	var html = Mustache.render(tpl, data, partials);
	//输出：
	<h1>Apple</h1>
	<ul><li>1 Infinite Loop Cupertino</br></li><li>California</li><li>CA</li><li>95014</li></ul>

## {{{keyName}}}
`{{keyName}}`输出会将特殊字符转译，如果想保持内容原样输出可以使用`{{{}}}`，例如：

	var tpl = '{{#address}} <p>{{{street}}}</p> {{/address}}';
	//输出：
	<p>1 Infinite Loop Cupertino</br></p>

## {{!comments}}
`!`表示注释，注释后不会渲染输出任何内容。 
	
	{{!这里是注释}}
	//输出：