> 英文文档地址：<https://eslint.org/docs/rules/semi>

`JavaScript`在众多的类C语言中是独一无二的，因为它不需要你在每个语句的末尾添加分号。在多数情况下，`JavaScript`引擎能够自行确定在某个位置是否需要分号，并在需要时为我们自动添加上分号。这一特性被称作自动分号插入（`automatic semicolon insertion`，简称`ASI`），并且成为了`JavaScript`中饱受争议的功能之一。例如，下面这两行声明语句都是有效的：

	var name = "ESLint"
	var website = "eslint.org";

在第一行，`JavaScript`引擎会自动在末尾插入分号，因此这不会被看成是一个语法错误。`JavaScript`引擎还知道如何解释该行并知道行末尾就是语句的结束。

在关于ASI的辩论中，通常有两种思想流派。其中一个流派认为：我们应该将ASI视为不存在，并且始终手动地为每一个语句添加上分号。理论依据是，始终都加上分号总比去尝试记住它们何时需要或不需要容易多了，从而降低了引入错误的可能性。

但是，对于喜欢使用分号的人来说，ASI机制有时会很棘手。例如，下面这段代码：

	return
	{
	    name: "ESLint"
	};

它可能字面上看起来更像是个返回一个对象的语句，但是，`JavaScript`引擎会将这段代码解释为：

	return;
	{
	    name: "ESLint";
	}

实际上，在return语句后面插入的分号，导致了它后面的代码永远无法访问，效果类似下面这段代码。

	function fn() {
	    x = 1;
	    return x;
	    x = 3; // this will never execute
	}

另外一个流派认为：既然`JavaScript`引擎会自动为我们插入分号，分号就是可选的，我们没必要去手动插入。但是，对于不使用分号的人来说，ASI机制也很棘手。例如，下面这段代码：

	var globalCounter = { }
	 
	(function () {
	    var n = 0
	    globalCounter.increment = function () {
	        return ++n
	    }
	})()

在此示例中，分号会被插入到第一行的末尾（换行符会结束一个语句，就跟分号的效果一样），从而导致出现运行时错误（因为像调用函数一样去调用了一个空对象）。

虽然ASI允许更加自由的编码风格，但它偶尔也会使你的代码以意想不到的方式运行，无论你是否使用分号。因此，最好知道ASI何时发生或者不发生，并且让ESLint保护你的代码避免出现这些潜在的意外情况。简而言之，正如`Isaac Schlueter`曾经描述的那样，`\n`换行符会结束一个语句，就跟分号的效果一样，除非满足下列条件之一：

- 语句含有未闭合的括号（例如：数组、对象）或以其他无效的结尾方式（例如：以","或"."结尾）结束。
- 该行是--或++（在这种情况下，它将递减/递增下一个标记）。
- 该行是一个 for()，while()，do-if()，或 else 语句，并且没有跟上 “{” 字符。
- 下一行以 “[”，“(”，“+”，“*”，“/”，“-”，“,”，“.”，或只能出现在同一个表达式中的两个标记之间一些其他二进制运算符开头。

# 规则细节
此规则强制使用一致的分号书写风格。

## 选项
此规则有两个选项，字符串选项和对象选项。

**字符串选项：**

- `always`（默认值）在语句结尾处需要分号
- `never`不允许分号作为语句的末尾（不包括那些为了消除歧义以 [，(，/，+，或 - 开头的语句）

**对象选项（应用`always`选项时可用）：**
 
- `omitLastInOneLineBlock`: true 当语句块中的括号（包含括号中的内容）在同一行时， 忽略块中的最后一个分号。

**对象选项（应用 "never" 选项时可用）：**

- `beforeStatementContinuationChars`:`any`（默认）如果下一行语句以 [，(，/，+，或 - 开头，忽略语句末尾的分号（或缺失分号），
- `beforeStatementContinuationChars`: `always` 如果下一行语句以 [，(，/，+，或 - 开头，在语句末尾需要添加分号。
- `beforeStatementContinuationChars`: `never` 如果该语句不会因为ASI而带来风险，那么即使它的下一行语句以 [，(，/，+，或 - 开头，也不允许在语句末尾添加分号。

### always
使用`always`规则的错误代码示例：

	/*eslint semi: ["error", "always"]*/
	 
	var name = "ESLint"
	 
	object.method = function() {
	    // ...
	}

使用`always`规则的正确代码示例：

	/*eslint semi: "error"*/
	 
	var name = "ESLint";
	 
	object.method = function() {
	    // ...
	};

### never
使用`never`规则的错误代码示例：

	/*eslint semi: ["error", "never"]*/
	 
	var name = "ESLint";
	 
	object.method = function() {
	    // ...
	};

使用`never`规则的正确代码示例：

	/*eslint semi: ["error", "never"]*/
	 
	var name = "ESLint"
	 
	object.method = function() {
	    // ...
	}
	 
	var name = "ESLint"
	 
	;(function() {
	    // ...
	})()
	 
	import a from "a"
	(function() {
	    // ...
	})()
	 
	import b from "b"
	;(function() {
	    // ...
	})()

### omitLastInOneLineBlock
使用`always, { "omitLastInOneLineBlock": true } `选项的正确代码示例：

	/*eslint semi: ["error", "always", { "omitLastInOneLineBlock": true}] */
	 
	if (foo) { bar() }
	 
	if (foo) { bar(); baz() }

### beforeStatementContinuationChars
使用`never, { "beforeStatementContinuationChars": "always" }`选项的错误代码示例：

	/*eslint semi: ["error", "never", { "beforeStatementContinuationChars": "always"}] */
	import a from "a"
	 
	(function() {
	    // ...
	})()

使用 `never, { "beforeStatementContinuationChars": "never" }` 选项的错误代码示例：

	/*eslint semi: ["error", "never", { "beforeStatementContinuationChars": "never"}] */
	import a from "a"
	 
	;(function() {
	    // ...
	})()