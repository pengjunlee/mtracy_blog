> 英文文档地址：<https://eslint.org/docs/rules/indent>

嵌套的代码块或者语句需要具有一定的缩进，类似下面这样：

	function hello(indentSize, type) {
	    if (indentSize === 4 && type !== 'tab') {
	        console.log('Each next indentation will increase on 4 spaces');
	    }
	}

以下是一些不同代码风格所推荐的常见缩进方案：

- 两个空格，不要再长了，也不要使用`Tab`作缩进：Google，npm，Node.js，Idiomatic，Felix
- 使用`Tab`作缩进：jQuery
- 四个空格：Crockford

# 规则细节
此规则强制执行一致的缩进样式。默认样式是缩进四个空格。

## 选项
此规则有一个混合选项。

例如，使用两个空格作缩进：

	{
	    "indent": ["error", 2]
	}

或者用 Tab 作缩进：

	{
	    "indent": ["error", "tab"]
	}

使用默认规则（缩进四个空格）的错误代码示例：

	/*eslint indent: "error"*/
	 
	if (a) {
	  b=c;
	  function foo(d) {
	    e=f;
	  }
	}

使用默认规则（缩进四个空格）的正确代码示例：

	/*eslint indent: "error"*/
	 
	if (a) {
	    b=c;
	    function foo(d) {
	        e=f;
	    }
	}

此规则有一个对象选项：

- `SwitchCase`（默认：0）指定`switch-case`语句的缩进级别
- `VariableDeclarator`（默认值：1）指定`var`变量声明语句的缩进级别;你也可以分别为`var，let 和 const 语句指定不同的缩进规则。该值也可以设置为 `first`，表明所有声明的变量都应与第一个声明的变量对齐。
- `outerIIFEBody` （默认值：1）指定 `IIFE（立即调用的函数表达式）`的缩进级别。
- `MemberExpression`（默认值：1）指定多行属性链的缩进级别。这也可以设置为`off`禁用检查MemberExpression缩进。
- `FunctionDeclaration` 通过传入一个对象来指定函数声明的缩进规则。
parameters（默认值：1）指定函数声明中参数的缩进级别，可以是指示缩进级别的数字，也可以是`first`字符串，也可以设置为`off`禁用函数参数检查。
body （默认值：1）指定函数声明主体的缩进级别。
- `FunctionExpression` 通过传入一个对象来指定函数表达式的缩进规则。
parameters（默认值：1）指定函数表达式的参数的缩进规则，可以传入数字、`first`、`off`。
body （默认值：1）指定函数表达式的主体的缩进规则。
- `CallExpression` 通过传入一个对象来指定函数调用表达式的缩进规则。
arguments（默认值：1）指定调用表达式的参数的缩进规则，可以传入数字、`first`、`off`。
- `ArrayExpression`（默认值：1）指定数组中的元素的缩进级别，可以传入数字、`first`、`off`。
- `ObjectExpression`（默认值：1）指定对象中的属性的缩进级别，可以传入数字、`first`、`off`。
- `ImportDeclaration`（默认值：1）指定 import 语句的缩进级别，可以传入数字、`first`、`off`。
- `flatTernaryExpressions`（默认值：false）指定是否需要缩进嵌套在其他三元表达式中的三元表达式。
- `ignoredNodes` 接受一个选择器数组。如果某个AST节点与其中的任何一个选择器匹配，则将忽略作为该节点的直接子节点的缩进标记。有时你也许并不希望去执行缩进来迫使你的代码满足一定的语法样式，`ignoredNodes` 可以用作放宽限制规则的一个后门。
- `ignoreComments`（默认值：false）指定注释是否需要需要与前一行或下一行的节点对齐。

示例 
下面列举了一些选项的用法与代码缩进示例。

## Tab

	/*eslint indent: ["error", "tab"]*/
	 
	if (a) {
	/*tab*/b=c;
	/*tab*/function foo(d) {
	/*tab*//*tab*/e=f;
	/*tab*/}
	}

## SwitchCase

	/*eslint indent: ["error", 2, { "SwitchCase": 1 }]*/
	 
	switch(a){
	  case "a":
	    break;
	  case "b":
	    break;
	}

## VariableDeclarator

	/*eslint indent: ["error", 2, { "VariableDeclarator": 1 }]*/
	/*eslint-env es6*/
	 
	var a,
	  b,
	  c;
	let a,
	  b,
	  c;
	const a = 1,
	  b = 2,
	  c = 3;
	/*eslint indent: ["error", 2, { "VariableDeclarator": 2 }]*/
	/*eslint-env es6*/
	 
	var a,
	    b,
	    c;
	let a,
	    b,
	    c;
	const a = 1,
	    b = 2,
	    c = 3;
	/*eslint indent: ["error", 2, { "VariableDeclarator": "first" }]*/
	/*eslint-env es6*/
	 
	var a,
	    b,
	    c;
	let a,
	    b,
	    c;
	const a = 1,
	      b = 2,
	      c = 3;
	/*eslint indent: ["error", 2, { "VariableDeclarator": { "var": 2, "let": 2, "const": 3 } }]*/
	/*eslint-env es6*/
	 
	var a,
	    b,
	    c;
	let a,
	    b,
	    c;
	const a = 1,
	      b = 2,
	      c = 3;

## outerIIFEBody

	/*eslint indent: ["error", 2, { "outerIIFEBody": 0 }]*/
	 
	(function() {
	 
	function foo(x) {
	  return x + 1;
	}
	 
	})();
	 
	 
	if(y) {
	   console.log('foo');
	}

## MemberExpression

	/*eslint indent: ["error", 2, { "MemberExpression": 1 }]*/
	 
	foo
	  .bar
	  .baz();

## FunctionDeclaration

	/*eslint indent: ["error", 2, { "FunctionDeclaration": {"body": 1, "parameters": 2} }]*/
	 
	function foo(bar,
	    baz,
	    qux) {
	  qux();
	}
	/*eslint indent: ["error", 2, {"FunctionDeclaration": {"parameters": "first"}}]*/
	 
	function foo(bar, baz,
	             qux, boop) {
	  qux();
	}

## FunctionExpression

	/*eslint indent: ["error", 2, { "FunctionExpression": {"body": 1, "parameters": 2} }]*/
	 
	var foo = function(bar,
	    baz,
	    qux) {
	  qux();
	}
	/*eslint indent: ["error", 2, {"FunctionExpression": {"parameters": "first"}}]*/
	 
	var foo = function(bar, baz,
	                   qux, boop) {
	  qux();
	}

## CallExpression
	/*eslint indent: ["error", 2, { "CallExpression": {"arguments": 1} }]*/
	 
	foo(bar,
	  baz,
	  qux
	);
	/*eslint indent: ["error", 2, {"CallExpression": {"arguments": "first"}}]*/
	 
	foo(bar, baz,
	    baz, boop, beep);

## ArrayExpression 
	/*eslint indent: ["error", 2, { "ArrayExpression": 1 }]*/
	 
	var foo = [
	  bar,
	  baz,
	  qux
	];
	 
	
	/*eslint indent: ["error", 2, {"ArrayExpression": "first"}]*/
	 
	var foo = [bar,
	           baz,
	           qux
	];

## ObjectExpression

	/*eslint indent: ["error", 2, { "ObjectExpression": 1 }]*/
	 
	var foo = {
	  bar: 1,
	  baz: 2,
	  qux: 3
	};
	/*eslint indent: ["error", 2, {"ObjectExpression": "first"}]*/
	 
	var foo = { bar: 1,
	            baz: 2 };

## ImportDeclaration

	/*eslint indent: ["error", 4, { "ImportDeclaration": 1 }]*/
	 
	import { foo,
	    bar,
	    baz,
	} from 'qux';
	 
	import {
	    foo,
	    bar,
	    baz,
	} from 'qux';
	/*eslint indent: ["error", 4, { "ImportDeclaration": "first" }]*/
	 
	import { foo,
	         bar,
	         baz,
	} from 'qux';

## flatTernaryExpressions

	/*eslint indent: ["error", 4, { "flatTernaryExpressions": false }]*/
	 
	var a =
	    foo ? bar :
	        baz ? qux :
	            boop;
	/*eslint indent: ["error", 4, { "flatTernaryExpressions": true }]*/
	 
	var a =
	    foo ? bar :
	    baz ? qux :
	    boop;

## ignoredNodes

	/*eslint indent: ["error", 4, { "ignoredNodes": ["ConditionalExpression"] }]*/
	 
	var a = foo
	      ? bar
	      : baz;
	 
	var a = foo
	                ? bar
	: baz;
	/*eslint indent: ["error", 4, { "ignoredNodes": ["CallExpression > FunctionExpression.callee > BlockStatement.body"] }]*/
	 
	(function() {
	 
	foo();
	bar();
	 
	})

## ignoreComments

	/*eslint indent: ["error", 4, { "ignoreComments": true }] */
	 
	if (foo) {
	    doSomething();
	 
	// comment intentionally de-indented
	    doSomethingElse();
	}
 