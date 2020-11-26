正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

下表整理了一些正则表达式中经常用到的语法：
<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td>&nbsp;</td><td>语法</td><td>描述</td><td>表达式示例</td><td>匹配示例</td></tr><tr><td rowspan="13">字符</td><td>普通字符</td><td>匹配自身</td><td>abc</td><td>abc</td></tr><tr><td>\t</td><td>匹配一个制表符</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>\r</td><td>匹配一个回车符</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>\n</td><td>匹配一个换行符</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>\f</td><td>匹配一个换页符</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>\v</td><td>匹配一个垂直制表符</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>.</td><td>匹配任意除换行符'\n'外的字符<br> 在DOTALL模式中也能匹配换行符</td><td>a.c</td><td>abc</td></tr><tr><td>^</td><td>匹配输入字符串的开始位置</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>$</td><td>匹配输入字符串的结束位置</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>\b</td><td>匹配一个单词边界，即字与空格间的位置</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>\B</td><td>匹配非单词边界</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>|</td><td>指明两项之间的选择一个</td><td>&nbsp;</td><td>&nbsp;</td></tr><tr><td>[...]</td><td>字符集，对应的位置可以是字符集中任意字符。<br> 字符集中的字符可以逐个列出，也可以给出范围，例如 [abc]或者[a-c]。<br> 第一个字符如果是 ^ 则表示取反，如 [^abc]表示除abc外的任意字符。<br> 所有的特殊字符在字符集中都失去原有的特殊含义。在字符集中如果要使用 ]、- 、^，可以在前面加上反斜杠，或把 ] 、- 放在第一个字符，把 ^ 放在非第一个字符。</td><td>a[]a-z\[\-^]c</td><td>a[c<br> a]c<br> abc<br> a-c<br> a^c</td></tr><tr><td rowspan="6">预定义字符集</td><td>\d</td><td>数字：[0-9]</td><td>a\dc</td><td>a6c</td></tr><tr><td>\D</td><td>非数字：[^0-9] 或者[^\d]</td><td>a\Dc</td><td>abc</td></tr><tr><td>\s</td><td>空白字符：[空格\t\r\n\f\v]</td><td>a\sc</td><td>a c</td></tr><tr><td>\S</td><td>非空白字符：[^\s]</td><td>a\Sc</td><td>abc</td></tr><tr><td>\w</td><td>单词字符：[A-Za-z0-9_]</td><td>a\wc</td><td>a_c</td></tr><tr><td>\W</td><td>非单词字符：[^\w]</td><td>a\Wc</td><td>a c</td></tr><tr><td rowspan="4">数量词</td><td>*</td><td>匹配前一个字符任意次（0次、1次、...）</td><td>abc*d</td><td>abd<br> abccd</td></tr><tr><td>+</td><td>匹配前一个字符1次或多次</td><td>abc+d</td><td>abcd<br> abccd</td></tr><tr><td>?</td><td>匹配前一个字符0次或1次</td><td>abc?d</td><td>abd<br> abcd</td></tr><tr><td>{n}<br> {n,}<br> {n,m}</td><td>匹配前一个字符n次<br> 匹配前一个字符至少n次&nbsp;&nbsp;<br> 匹配前一个字符至少n次，最多m次</td><td>abc{3}d</td><td>abcccd</td></tr></tbody></table>

<font color=red>注意</font>：`*`、`+`限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个`?`就可以实现非贪婪或最小匹配。

Python自1.5版本起增加了`re`模块，使得Python语言拥有全部的正则表达式功能，常用正则表达式的方法有如下几个：

	re.compile(pattern, flags=0) # 编译，获取一个正则表达式对象用于匹配和替换
	pattern.match() # 对整个字符串进行匹配
	pattern.search() # 找一个
	pattern.findall() # 找所有
	pattern.sub() # 替换

`compile()`方法中的`flags`参数用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。

<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td>re.I</td><td>使匹配对大小写不敏感</td></tr><tr><td>re.L</td><td>做本地化识别（locale-aware）匹配</td></tr><tr><td>re.M</td><td>多行匹配，影响 ^ 和 $</td></tr><tr><td>re.S</td><td>使 . 匹配包括换行在内的所有字符</td></tr><tr><td>re.U</td><td>根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.</td></tr><tr><td>re.X</td><td>该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。</td></tr></tbody></table>

# match方法
match方法尝试从字符串的起始位置与模式进行匹配，如果匹配成功的话，会返回一个包含了所有匹配分组的结果对象，否则返回None。 

	import re
	 
	match_str = 'Cats are smarter than dogs'
	match_pattern = re.compile(r'(.*) are (.*?) .*', re.M | re.I)
	match_ret = match_pattern.match(match_str)
	 
	if match_ret:
	    print("match_ret.group(0) : ", match_ret.group(0))
	    print("match_ret.group(1) : ", match_ret.group(1))
	    print("match_ret.group(2) : ", match_ret.group(2))
	else:
	    print("No match!!")
	 
	print(match_ret.groups())

程序打印结果：

	match_ret.group(0) :  Cats are smarter than dogs
	match_ret.group(1) :  Cats
	match_ret.group(2) :  smarter
	('Cats', 'smarter')

# search方法
`search()`方法会从左至右扫描整个字符串并返回第一个与模式匹配成功的部分字符串。

	import re
	 
	search_str = 'Big fish eat small fish, small fish eat shrimp'
	search_pattern = re.compile(r'(.*) fish .*', re.M | re.I)
	search_ret = search_pattern.match(search_str)
	 
	if search_ret:
	    print("search_ret.group(0) : ", search_ret.group(0))
	    print("search_ret.group(1) : ", search_ret.group(1))
	else:
	    print("No search!!")

 程序打印结果：

	search_ret.group(0) :  Big fish eat small fish, small fish eat shrimp
	search_ret.group(1) :  Big fish eat small fish, small

# findall方法
`findall()`方法会在字符串中找到与模式匹配的所有子串，并返回一个列表，如果没有找到匹配的字符串，则返回空列表。

	import re
	 
	find_str = 'Big fish eat small fish, small fish eat shrimp'
	find_pattern = re.compile(r'(\S* fish)', re.M | re.I)
	find_ret = find_pattern.findall(find_str)
	print(find_ret)

程序打印结果： 

	['Big fish', 'small fish', 'small fish']

另外还有一个finditer方法和findall很类似，也是在字符串中找到正则表达式所匹配的所有子串，但是会把它们作为一个迭代器返回。

	find_str = 'Big fish eat small fish, small fish eat shrimp'
	find_pattern = re.compile(r'(\S* fish)', re.M | re.I)
	 
	 
	iter = find_pattern.finditer(find_str)
	for match in iter:
	    print(match.group())

# sub方法
`sub()`方法用来使用正则表达式对与模式匹配的子串进行替换，可以通过count参数来指定替换次数。

	import re
	 
	sub_str = 'Big fish eat small fish, small fish eat shrimp'
	sub_pattern = re.compile(r'(\S* fish)', re.M | re.I)
	sub_ret = sub_pattern.sub('fish', sub_str, count=3)
	print(sub_ret)

程序打印结果： 

	fish eat fish, fish eat shrimp

与sub方法类似地，有一个split方法能够利用与正则模式匹配的子串将字符串进行分割并将分割后的结果放入列表返回。

	split_str = 'Big fish eat small fish, small fish eat shrimp'
	split_pattern = re.compile(r'(\S* fish)', re.M | re.I)
	split_ret = split_pattern.split(sub_str,maxsplit=0)
	print(split_ret)

程序打印结果： 

	['', 'Big fish', ' eat ', 'small fish', ', ', 'small fish', ' eat shrimp']
 