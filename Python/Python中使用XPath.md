> 摘自W3School官方文档：<http://www.w3school.com.cn/xpath/index.asp>

# XPath简介
`XPath（XML Path Language）`是一门在HTML\XML文档中查找信息的语言，可用来在HTML\XML文档中对元素和属性进行遍历。在Python爬虫中，我们可以利用XPath快速地定位HTML\XML响应中的特定元素以及获取节点的信息，并且通常情况下会比使用正则表达式提取更简单而且更高效。

Chrome插件`XPath Helper`

	网盘链接：https://pan.baidu.com/s/1MHw9t5oQuFtH94Zox_bLkg
	提取码：4c5e

<font color=red>提示</font>：在使用XPath Helper选择标签时，被选中的标签会自动添加属性`class="xh-highlight"`。

# XPath语法
我们将以下面的这个XML文档为例对XPath的语法进行示例。

	<?xml version="1.0" encoding="ISO-8859-1"?>
	<bookstore>
	    <book>
	        <title lang="eng">Harry Potter</title>
	        <price>29.99</price>
	    </book>
	    <book>
	        <title lang="eng">Learning XML</title>
	        <price>39.95</price>
	    </book>
	</bookstore>

## 选取节点
XPath使用路径表达式来选取XML文档中的节点或节点集。节点是通过沿着路径 (path) 或者步 (step) 来选取的。

下面列出了最有用的路径表达式：

<table><tbody><tr><td> <p>表达式</p> </td><td> <p>描述</p> </td></tr><tr><td> <p>nodename</p> </td><td> <p>选取所有nodename子节点。</p> </td></tr><tr><td> <p>/</p> </td><td> <p>从根节点选取。</p> </td></tr><tr><td> <p>//</p> </td><td> <p>从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。</p> </td></tr><tr><td> <p>.</p> </td><td> <p>选取当前节点。</p> </td></tr><tr><td> <p>..</p> </td><td> <p>选取当前节点的父节点。</p> </td></tr><tr><td> <p>@</p> </td><td> <p>选取属性。</p> </td></tr><tr><td> <p>text()</p> </td><td> <p>选取文本。</p> </td></tr></tbody></table>

下面是一些实例：

<table><tbody><tr><td> <p>表达式</p> </td><td> <p>描述</p> </td></tr><tr><td> <p>nodename</p> </td><td> <p>选取所有nodename子节点。</p> </td></tr><tr><td> <p>/</p> </td><td> <p>从根节点选取。</p> </td></tr><tr><td> <p>//</p> </td><td> <p>从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。</p> </td></tr><tr><td> <p>.</p> </td><td> <p>选取当前节点。</p> </td></tr><tr><td> <p>..</p> </td><td> <p>选取当前节点的父节点。</p> </td></tr><tr><td> <p>@</p> </td><td> <p>选取属性。</p> </td></tr><tr><td> <p>text()</p> </td><td> <p>选取文本。</p> </td></tr></tbody></table>

## 谓语（Predicates）
谓语用来查找某个特定的节点或者包含某个指定的值的节点。谓语被嵌在方括号中。

<table><tbody><tr><td> <p>路径表达式</p> </td><td> <p>结果</p> </td></tr><tr><td> <p>/bookstore/book[1]</p> </td><td> <p>选取属于 bookstore 子元素的第一个 book 元素。</p> </td></tr><tr><td> <p>/bookstore/book[last()]</p> </td><td> <p>选取属于 bookstore 子元素的最后一个 book 元素。</p> </td></tr><tr><td> <p>/bookstore/book[last()-1]</p> </td><td> <p>选取属于 bookstore 子元素的倒数第二个 book 元素。</p> </td></tr><tr><td> <p>/bookstore/book[position()&lt;3]</p> </td><td> <p>选取最前面的两个属于 bookstore 元素的子元素的 book 元素。</p> </td></tr><tr><td> <p>//title[@lang]</p> </td><td> <p>选取所有拥有名为 lang 的属性的 title 元素。</p> </td></tr><tr><td> <p>//title[@lang='eng']</p> </td><td> <p>选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。</p> </td></tr><tr><td> <p>/bookstore/book[price&gt;35.00]</p> </td><td> <p>选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。</p> </td></tr><tr><td> <p>/bookstore/book[price&gt;35.00]/title</p> </td><td> <p>选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。</p> </td></tr></tbody></table>

## 选取未知节点
XPath通配符可用来选取未知的XML元素。

<table><tbody><tr><td> <p>通配符</p> </td><td> <p>描述</p> </td></tr><tr><td> <p>*</p> </td><td> <p>匹配任何元素节点。</p> </td></tr><tr><td> <p>@*</p> </td><td> <p>匹配任何属性节点。</p> </td></tr><tr><td> <p>node()</p> </td><td> <p>匹配任何类型的节点。</p> </td></tr></tbody></table>

下面是一些实例：

<table><tbody><tr><td> <p>路径表达式</p> </td><td> <p>结果</p> </td></tr><tr><td> <p>/bookstore/*</p> </td><td> <p>选取 bookstore 元素的所有子元素。</p> </td></tr><tr><td> <p>//*</p> </td><td> <p>选取文档中的所有元素。</p> </td></tr><tr><td> <p>//title[@*]</p> </td><td> <p>选取所有带有属性的 title 元素。</p> </td></tr></tbody></table>

## 选取若干路径
通过在路径表达式中使用`|`运算符，您可以选取若干个路径。

<table><tbody><tr><td> <p>路径表达式</p> </td><td> <p>结果</p> </td></tr><tr><td> <p>//book/title | //book/price</p> </td><td> <p>选取 book 元素的所有 title 和 price 元素。</p> </td></tr><tr><td> <p>//title | //price</p> </td><td> <p>选取文档中的所有 title 和 price 元素。</p> </td></tr><tr><td> <p>/bookstore/book/title | //price</p> </td><td> <p>选取属于 bookstore 元素的 book 元素的所有 title 元素，以及文档中所有的 price 元素。</p> </td></tr></tbody></table>

## XPath轴
轴可定义相对于当前节点的节点集。

<table><tbody><tr><td> <p>轴名称</p> </td><td> <p>结果</p> </td></tr><tr><td> <p>ancestor</p> </td><td> <p>选取当前节点的所有先辈（父、祖父等）。</p> </td></tr><tr><td> <p>ancestor-or-self</p> </td><td> <p>选取当前节点的所有先辈（父、祖父等）以及当前节点本身。</p> </td></tr><tr><td> <p>attribute</p> </td><td> <p>选取当前节点的所有属性。</p> </td></tr><tr><td> <p>child</p> </td><td> <p>选取当前节点的所有子元素。</p> </td></tr><tr><td> <p>descendant</p> </td><td> <p>选取当前节点的所有后代元素（子、孙等）。</p> </td></tr><tr><td> <p>descendant-or-self</p> </td><td> <p>选取当前节点的所有后代元素（子、孙等）以及当前节点本身。</p> </td></tr><tr><td> <p>following</p> </td><td> <p>选取文档中当前节点的结束标签之后的所有节点。</p> </td></tr><tr><td> <p>namespace</p> </td><td> <p>选取当前节点的所有命名空间节点。</p> </td></tr><tr><td> <p>parent</p> </td><td> <p>选取当前节点的父节点。</p> </td></tr><tr><td> <p>preceding</p> </td><td> <p>选取文档中当前节点的开始标签之前的所有节点。</p> </td></tr><tr><td> <p>preceding-sibling</p> </td><td> <p>选取当前节点之前的所有同级节点。</p> </td></tr><tr><td> <p>self</p> </td><td> <p>选取当前节点。</p> </td></tr></tbody></table>

下面是一些实例：

<table><tbody><tr><td> <p>例子</p> </td><td> <p>结果</p> </td></tr><tr><td> <p>child::book</p> </td><td> <p>选取所有属于当前节点的子元素的 book 节点。</p> </td></tr><tr><td> <p>attribute::lang</p> </td><td> <p>选取当前节点的 lang 属性。</p> </td></tr><tr><td> <p>child::*</p> </td><td> <p>选取当前节点的所有子元素。</p> </td></tr><tr><td> <p>attribute::*</p> </td><td> <p>选取当前节点的所有属性。</p> </td></tr><tr><td> <p>child::text()</p> </td><td> <p>选取当前节点的所有文本子节点。</p> </td></tr><tr><td> <p>child::node()</p> </td><td> <p>选取当前节点的所有子节点。</p> </td></tr><tr><td> <p>descendant::book</p> </td><td> <p>选取当前节点的所有 book 后代。</p> </td></tr><tr><td> <p>ancestor::book</p> </td><td> <p>选择当前节点的所有 book 先辈。</p> </td></tr><tr><td> <p>ancestor-or-self::book</p> </td><td> <p>选取当前节点的所有 book 先辈以及当前节点（如果此节点是 book 节点）</p> </td></tr><tr><td> <p>child::*/child::price</p> </td><td> <p>选取当前节点的所有 price 孙节点。</p> </td></tr></tbody></table>

## XPath 运算符
下面列出了可用在XPath表达式中的运算符：

<table><tbody><tr><td> <p>运算符</p> </td><td> <p>描述</p> </td><td> <p>实例</p> </td><td> <p>返回值</p> </td></tr><tr><td> <p>|</p> </td><td> <p>计算两个节点集</p> </td><td> <p>//book | //cd</p> </td><td> <p>返回所有拥有 book 和 cd 元素的节点集</p> </td></tr><tr><td> <p>+</p> </td><td> <p>加法</p> </td><td> <p>6 + 4</p> </td><td> <p>10</p> </td></tr><tr><td> <p>-</p> </td><td> <p>减法</p> </td><td> <p>6 - 4</p> </td><td> <p>2</p> </td></tr><tr><td> <p>*</p> </td><td> <p>乘法</p> </td><td> <p>6 * 4</p> </td><td> <p>24</p> </td></tr><tr><td> <p>div</p> </td><td> <p>除法</p> </td><td> <p>8 div 4</p> </td><td> <p>2</p> </td></tr><tr><td> <p>=</p> </td><td> <p>等于</p> </td><td> <p>price=9.80</p> </td><td> <p>如果 price 是 9.80，则返回 true。</p> <p>如果 price 是 9.90，则返回 false。</p> </td></tr><tr><td> <p>!=</p> </td><td> <p>不等于</p> </td><td> <p>price!=9.80</p> </td><td> <p>如果 price 是 9.90，则返回 true。</p> <p>如果 price 是 9.80，则返回 false。</p> </td></tr><tr><td> <p>&lt;</p> </td><td> <p>小于</p> </td><td> <p>price&lt;9.80</p> </td><td> <p>如果 price 是 9.00，则返回 true。</p> <p>如果 price 是 9.90，则返回 false。</p> </td></tr><tr><td> <p>&lt;=</p> </td><td> <p>小于或等于</p> </td><td> <p>price&lt;=9.80</p> </td><td> <p>如果 price 是 9.00，则返回 true。</p> <p>如果 price 是 9.90，则返回 false。</p> </td></tr><tr><td> <p>&gt;</p> </td><td> <p>大于</p> </td><td> <p>price&gt;9.80</p> </td><td> <p>如果 price 是 9.90，则返回 true。</p> <p>如果 price 是 9.80，则返回 false。</p> </td></tr><tr><td> <p>&gt;=</p> </td><td> <p>大于或等于</p> </td><td> <p>price&gt;=9.80</p> </td><td> <p>如果 price 是 9.90，则返回 true。</p> <p>如果 price 是 9.70，则返回 false。</p> </td></tr><tr><td> <p>or</p> </td><td> <p>或</p> </td><td> <p>price=9.80 or price=9.70</p> </td><td> <p>如果 price 是 9.80，则返回 true。</p> <p>如果 price 是 9.50，则返回 false。</p> </td></tr><tr><td> <p>and</p> </td><td> <p>与</p> </td><td> <p>price&gt;9.00 and price&lt;9.90</p> </td><td> <p>如果 price 是 9.80，则返回 true。</p> <p>如果 price 是 8.50，则返回 false。</p> </td></tr><tr><td> <p>mod</p> </td><td> <p>计算除法的余数</p> </td><td> <p>5 mod 2</p> </td><td> <p>1</p> </td></tr></tbody></table>

# 使用lxml
lxml是Python的一个第三方解析库，支持HTML和XML解析，而且效率非常高，弥补了Python自带的xml标准库在XML解析方面的不足。

由于是第三方库，所以在使用lxml之前需要先安装：

	pip install lxml

下面是一段示例代码

	# coding=utf-8
	 
	from lxml import etree
	 
	xml_data = '''
	<bookstore>
	<book>
	  <title lang="eng">Harry Potter</title>
	  <price>29.99</price>
	</book>
	<book>
	  <title lang="eng">Learning XML</title>
	  <price>39.95</price>
	</book>
	</bookstore>
	'''
	 
	# etree.HTML()可以接收str或者bytes类型数据，将其转化成 Element 对象
	html = etree.HTML(xml_data)
	# 从文件加载 HTML
	# html = etree.parse('test.html',etree.HTMLParser())
	 
	# lxml 会自动修 HTML ，查看一下 lxml 修正后的结果
	print(etree.tostring(html, pretty_print=True).decode('utf-8'))
	 
	# 获取 Learning XML 这本书的价格
	ret = html.xpath('//title[text()="Learning XML"]/following::price/text()')[0] if len(
	    html.xpath('//title[text()="Learning XML"]/following::price/text()')) else None
	 
	print(ret)

程序运行结果：

	<html>
	  <body><bookstore>
	 
	<book>
	  <title lang="eng">Harry Potter</title>
	  <price>29.99</price>
	</book>
	 
	<book>
	  <title lang="eng">Learning XML</title>
	  <price>39.95</price>
	</book>
	 
	</bookstore>
	</body>
	</html>
	 
	39.95

更多`lxml API`可以参考：<https://lxml.de/api/index.html>