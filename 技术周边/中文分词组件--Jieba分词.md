> 原文地址：<https://github.com/fxsjy/jieba>

# jieba简介
> `Jieba`(Chinese for "to stutter") Chinese text segmentation: built to be the best Python Chinese word segmentation module.

翻译成中文：`结巴`中文分词：做最好的`Python`中文分词组件

## 组件特点

- 支持繁体分词
- 支持自定义词典
- MIT 授权协议
- 支持三种分词模式：

	- 精确模式，试图将句子最精确地切开，适合文本分析；
	- 全模式，把句子中所有的可以成词的词语都扫描出来, 速度非常快，但是不能解决歧义；
	- 搜索引擎模式，在精确模式的基础上，对长词再次切分，提高召回率，适合用于搜索引擎分词。


## 安装方法

- 全自动安装：`easy_install jieba`或者`pip install jieba`/`pip3 install jieba`
- 半自动安装：先下载`http://pypi.python.org/pypi/jieba/`，解压后运行`python setup.py install`
- 手动安装：将`jieba`目录放置于当前目录或者`site-packages`目录

jieba语法对Python2和Python3均兼容，通过`import jieba`来引入。

## 算法

- 基于前缀词典实现高效的词图扫描，生成句子中汉字所有可能成词情况所构成的有向无环图 (DAG)；
- 采用了动态规划查找最大概率路径, 找出基于词频的最大切分组合；
- 对于未登录词，采用了基于汉字成词能力的 HMM 模型，使用了 Viterbi 算法；

# 使用jieba
## 分词

- `jieba.cut`方法接受三个输入参数: 需要分词的字符串；cut_all 参数用来控制是否采用全模式；HMM 参数用来控制是否使用 HMM 模型；
- `jieba.cut_for_search`方法接受两个参数：需要分词的字符串；是否使用 HMM 模型。该方法适合用于搜索引擎构建倒排索引的分词，粒度比较细，待分词的字符串可以是`unicode`或`UTF-8`字符串、GBK 字符串。注意：不建议直接输入 `GBK`字符串，可能无法预料地错误解码成`UTF-8`。
- `jieba.cut`以及`jieba.cut_for_search`返回的结构都是一个可迭代的`generator`，可以使用`for`循环来获得分词后得到的每一个词语(unicode)，或者用`jieba.lcut`以及`jieba.lcut_for_search`直接返回 list。
- `jieba.Tokenizer(dictionary=DEFAULT_DICT)`新建自定义分词器，可用于同时使用不同词典。`jieba.dt`为默认分词器，所有全局分词相关函数都是该分词器的映射。

代码示例：

	# encoding=utf-8
	import jieba
	 
	seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
	print("全模式: " + "/ ".join(seg_list))  # 全模式
	 
	seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
	print("精确模式: " + "/ ".join(seg_list))  # 精确模式
	 
	seg_list = jieba.cut("他来到了网易杭研大厦")  # 默认是精确模式
	print("默认精确模式: " + ", ".join(seg_list))
	 
	seg_list = jieba.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式
	print("搜索引擎模式: " + ", ".join(seg_list))

输出：

	全模式: 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学
	精确模式: 我/ 来到/ 北京/ 清华大学
	默认精确模式: 他, 来到, 了, 网易, 杭研, 大厦
	搜索引擎模式: 小明, 硕士, 毕业, 于, 中国, 科学, 学院, 科学院, 中国科学院, 计算, 计算所, ，, 后, 在, 日本, 京都, 大学, 日本京都大学, 深造

## 添加自定义词典
### 载入词典
开发者可以指定自己自定义的词典，以便包含`jieba`词库里没有的词。虽然`jieba`有新词识别能力，但是自行添加新词可以保证更高的正确率。

> 用法： jieba.load_userdict(file_name)  # file_name 为文件类对象或自定义词典的路径

词典格式和`dict.txt`一样，一个词占一行；每一行分三部分：词语、词频（可省略）、词性（可省略），用空格隔开，顺序不可颠倒。`file_name`若为路径或二进制方式打开的文件，则文件必须为`UTF-8`编码。

词频省略时使用自动计算的能保证分出该词的词频。

例如，自定义的`userdict.txt`词典内容如下：

	云计算 5
	李小福 2 nr
	创新办 3 i
	easy_install 3 eng
	好用 300
	韩玉赏鉴 3 nz
	八一双鹿 3 nz
	台中
	凱特琳 nz
	Edu Trust认证 2000

Python 代码如下： 

	# encoding=utf-8
	import jieba
	import jieba.posseg as pseg
	 
	jieba.load_userdict("userdict.txt")
	 
	jieba.add_word('石墨烯')
	jieba.add_word('凱特琳')
	jieba.del_word('自定义词')
	 
	test_sent = (
	    "李小福是创新办主任也是云计算方面的专家; 什么是八一双鹿\n"
	    "例如我输入一个带“韩玉赏鉴”的标题，在自定义词库中也增加了此词为N类\n"
	    "「台中」正確應該不會被切開。mac上可分出「石墨烯」；此時又可以分出來凱特琳了。"
	)
	 
	words = jieba.cut(test_sent)
	print('/'.join(words))
	 
	print("=" * 40)
	 
	result = pseg.cut(test_sent)
	 
	for w in result:
	    print(w.word, "/", w.flag, ", ", end=' ')
	 
	print("\n" + "=" * 40)
	 
	terms = jieba.cut('easy_install is great')
	print('/'.join(terms))
	terms = jieba.cut('python 的正则表达式是好用的')
	print('/'.join(terms))
	 
	print("=" * 40)
	# test frequency tune
	testlist = [
	    ('今天天气不错', ('今天', '天气')),
	    ('如果放到post中将出错。', ('中', '将')),
	    ('我们中出了一个叛徒', ('中', '出')),
	]
	 
	for sent, seg in testlist:
	    print('/'.join(jieba.cut(sent, HMM=False)))
	    word = ''.join(seg)
	    print('%s Before: %s, After: %s' % (word, jieba.get_FREQ(word), jieba.suggest_freq(seg, True)))
	    print('/'.join(jieba.cut(sent, HMM=False)))
	    print("-" * 40)

输出：

	李小福/是/创新办/主任/也/是/云计算/方面/的/专家/;/ /什么/是/八一双鹿/
	/例如/我/输入/一个/带/“/韩玉赏鉴/”/的/标题/，/在/自定义/词库/中/也/增加/了/此/词为/N/类/
	/「/台中/」/正確/應該/不會/被/切開/。/mac/上/可/分出/「/石墨烯/」/；/此時/又/可以/分出/來/凱特琳/了/。
	========================================
	李小福 / nr ,  是 / v ,  创新办 / i ,  主任 / b ,  也 / d ,  是 / v ,  云计算 / x ,  方面 / n ,  的 / uj ,  专家 / n ,  ; / x ,    / x ,  什么 / r ,  是 / v ,  八一双鹿 / nz ,  
	 / x ,  例如 / v ,  我 / r ,  输入 / v ,  一个 / m ,  带 / v ,  “ / x ,  韩玉赏鉴 / nz ,  ” / x ,  的 / uj ,  标题 / n ,  ， / x ,  在 / p ,  自定义 / l ,  词库 / n ,  中 / f ,  也 / d ,  增加 / v ,  了 / ul ,  此 / r ,  词 / n ,  为 / p ,  N / eng ,  类 / q ,  
	 / x ,  「 / x ,  台中 / s ,  」 / x ,  正確 / ad ,  應該 / v ,  不 / d ,  會 / v ,  被 / p ,  切開 / ad ,  。 / x ,  mac / eng ,  上 / f ,  可 / v ,  分出 / v ,  「 / x ,  石墨烯 / x ,  」 / x ,  ； / x ,  此時 / c ,  又 / d ,  可以 / c ,  分出 / v ,  來 / zg ,  凱特琳 / nz ,  了 / ul ,  。 / x ,  
	========================================
	easy_install/ /is/ /great
	python/ /的/正则表达式/是/好用/的
	========================================
	今天天气/不错
	今天天气 Before: 3, After: 0
	今天/天气/不错
	----------------------------------------
	如果/放到/post/中将/出错/。
	中将 Before: 763, After: 494
	如果/放到/post/中/将/出错/。
	----------------------------------------
	我们/中/出/了/一个/叛徒
	中出 Before: 3, After: 3
	我们/中/出/了/一个/叛徒
	----------------------------------------

更改分词器（默认为`jieba.dt`）的`tmp_dir`和`cache_file`属性，可分别指定缓存文件所在的文件夹及其文件名，用于受限的文件系统。

### 调整词典

- 使用`add_word(word, freq=None, tag=None)`和`del_word(word)`可在程序中动态修改词典。
- 使用`suggest_freq(segment, tune=True)`可调节单个词语的词频，使其能（或不能）被分出来。

<font color=red>注意</font>：自动计算的词频在使用 HMM 新词发现功能时可能无效。

代码示例：

	# encoding=utf-8
	import jieba
	 
	print('/'.join(jieba.cut('如果放到post中将出错。', HMM=False)))
	jieba.suggest_freq(('中', '将'), True)
	print('/'.join(jieba.cut('如果放到post中将出错。', HMM=False)))
	 
	print('/'.join(jieba.cut('「台中」正确应该不会被切开', HMM=False)))
	jieba.suggest_freq('台中', True)
	print('/'.join(jieba.cut('「台中」正确应该不会被切开', HMM=False)))

输出： 

	如果/放到/post/中将/出错/。
	如果/放到/post/中/将/出错/。
	「/台/中/」/正确/应该/不会/被/切开
	「/台中/」/正确/应该/不会/被/切开

## 关键词提取
### 基于 TF-IDF 算法的关键词抽取

	import jieba.analyse
	 
	jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())
	# sentence 为待提取的文本
	# topK 为返回几个 TF/IDF 权重最大的关键词，默认值为 20
	# withWeight 为是否一并返回关键词权重值，默认值为 False
	# allowPOS 仅包括指定词性的词，默认值为空，即不筛选
	 
	jieba.analyse.TFIDF(idf_path=None) # 新建 TFIDF 实例，idf_path 为 IDF 频率文件

代码示例 ，`extract_tags.py`：

	# encoding=utf-8
	 
	import sys
	 
	sys.path.append('../')
	 
	import jieba
	import jieba.analyse
	from optparse import OptionParser
	 
	USAGE = "usage: python extract_tags.py [file name] -k [top k]"
	 
	parser = OptionParser(USAGE)
	parser.add_option("-k", dest="topK")
	opt, args = parser.parse_args()
	 
	if len(args) < 1:
	    print(USAGE)
	    sys.exit(1)
	 
	file_name = args[0]
	 
	if opt.topK is None:
	    topK = 10
	else:
	    topK = int(opt.topK)
	    
	# 关键词提取所使用逆向文件频率（IDF）文本语料库可以切换成自定义语料库的路径
	# jieba.analyse.set_idf_path("../extra_dict/idf.txt.big")
	 
	# 关键词提取所使用停止词（Stop Words）文本语料库可以切换成自定义语料库的路径
	# jieba.analyse.set_stop_words("../extra_dict/stop_words.txt")
	 
	content = open(file_name, 'rb').read()
	tags = jieba.analyse.extract_tags(content, topK=topK)
	print(",".join(tags))

### 基于 TextRank 算法的关键词抽取

	jieba.analyse.textrank(sentence, topK=20, withWeight=False, allowPOS=('ns', 'n', 'vn', 'v')) # 直接使用，接口相同，注意默认过滤词性。
	jieba.analyse.TextRank() # 新建自定义 TextRank 实例

基本思想:

- 将待抽取关键词的文本进行分词
- 以固定窗口大小(默认为5，通过span属性调整)，词之间的共现关系，构建图
- 计算图中节点的PageRank，注意是无向带权图

<br/>

	import jieba.analyse
	 
	s = "此外，公司拟对全资子公司吉林欧亚置业有限公司增资4.3亿元，增资后，吉林欧亚置业注册资本由7000万元增加到5亿元。吉林欧亚置业主要经营范围为房地产开发及百货零售等业务。目前在建吉林欧亚城市商业综合体项目。2013年，实现营业收入0万元，实现净利润-139.13万元。"
	 
	for x, w in jieba.analyse.textrank(s, withWeight=True):
	    print('%s %s' % (x, w))

## 词性标注

	# 新建自定义分词器，tokenizer 参数可指定内部使用的 jieba.Tokenizer 分词器。
	# jieba.posseg.dt 为默认词性标注分词器。
	jieba.posseg.POSTokenizer(tokenizer=None)

标注句子分词后每个词的词性，采用和 ictclas 兼容的标记法。

用法示例：

	# encoding=utf-8
	import jieba.posseg
	 
	words = jieba.posseg.cut("我爱北京天安门")
	for word, flag in words:
	    print('%s %s' % (word, flag))

输出：

	我 r
	爱 v
	北京 ns
	天安门 ns

## 并行分词
> 原理：将目标文本按行分隔后，把各行文本分配到多个 Python 进程并行分词，然后归并结果，从而获得分词速度的可观提升。

基于`python`自带的`multiprocessing`模块，目前暂不支持`Windows`。

用法：

	jieba.enable_parallel(4) # 开启并行分词模式，参数为并行进程数
	jieba.disable_parallel() # 关闭并行分词模式

示例：

	# encoding=utf-8
	import sys
	import time
	sys.path.append("../../")
	import jieba
	 
	jieba.enable_parallel()
	 
	url = sys.argv[1]
	content = open(url,"rb").read()
	t1 = time.time()
	words = "/ ".join(jieba.cut(content))
	 
	t2 = time.time()
	tm_cost = t2-t1
	 
	log_f = open("1.log","wb")
	log_f.write(words.encode('utf-8'))
	 
	print('speed %s bytes/second' % (len(content)/tm_cost))

<font color=red>实验结果</font>：在4核3.4GHz Linux机器上，对金庸全集进行精确分词，获得了1MB/s的速度，是单进程版的3.3倍。

<font color=red>注意</font>：并行分词仅支持默认分词器`jieba.dt`和`jieba.posseg.dt`。

## Tokenize
返回词语在原文的起止位置，注意，输入参数只接受`unicod`。

### 默认模式

	# encoding=utf-8
	import jieba
	 
	result1 = jieba.tokenize(u'永和服装饰品有限公司')
	for tk in result1:
	    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))
	结果：
	
	word 永和		 start: 0 		 end:2
	word 服装		 start: 2 		 end:4
	word 饰品		 start: 4 		 end:6
	word 有限公司		 start: 6 		 end:10

### 搜索模式

	# encoding=utf-8
	import jieba
	 
	result2 = jieba.tokenize(u'永和服装饰品有限公司', mode='search')
	for tk in result2:
	    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))

结果：

	word 永和		 start: 0 		 end:2
	word 服装		 start: 2 		 end:4
	word 饰品		 start: 4 		 end:6
	word 有限		 start: 6 		 end:8
	word 公司		 start: 8 		 end:10
	word 有限公司		 start: 6 		 end:10

## ChineseAnalyzer for Whoosh 搜索引擎
	# -*- coding: UTF-8 -*-
	from __future__ import unicode_literals
	import sys,os
	sys.path.append("../")
	from whoosh.index import create_in,open_dir
	from whoosh.fields import *
	from whoosh.qparser import QueryParser
	 
	from jieba.analyse.analyzer import ChineseAnalyzer
	 
	analyzer = ChineseAnalyzer()
	 
	schema = Schema(title=TEXT(stored=True), path=ID(stored=True), content=TEXT(stored=True, analyzer=analyzer))
	if not os.path.exists("tmp"):
	    os.mkdir("tmp")
	 
	ix = create_in("tmp", schema) # for create new index
	#ix = open_dir("tmp") # for read only
	writer = ix.writer()
	 
	writer.add_document(
	    title="document1",
	    path="/a",
	    content="This is the first document we’ve added!"
	)
	 
	writer.add_document(
	    title="document2",
	    path="/b",
	    content="The second one 你 中文测试中文 is even more interesting! 吃水果"
	)
	 
	writer.add_document(
	    title="document3",
	    path="/c",
	    content="买水果然后来世博园。"
	)
	 
	writer.add_document(
	    title="document4",
	    path="/c",
	    content="工信处女干事每月经过下属科室都要亲口交代24口交换机等技术性器件的安装工作"
	)
	 
	writer.add_document(
	    title="document4",
	    path="/c",
	    content="咱俩交换一下吧。"
	)
	 
	writer.commit()
	searcher = ix.searcher()
	parser = QueryParser("content", schema=ix.schema)
	 
	for keyword in ("水果世博园","你","first","中文","交换机","交换"):
	    print("result of ",keyword)
	    q = parser.parse(keyword)
	    results = searcher.search(q)
	    for hit in results:
	        print(hit.highlights("content"))
	    print("="*10)
	 
	for t in analyzer("我的好朋友是李明;我爱北京天安门;IBM和Microsoft; I have a dream. this is intetesting and interested me a lot"):
	    print(t.text)

结果：

	result of  水果世博园
	买<b class="match term0">水果</b>然后来<b class="match term1">世博园</b>
	==========
	result of  你
	second one <b class="match term0">你</b> 中文测试中文 is even more interesting
	==========
	result of  first
	<b class="match term0">first</b> document we’ve added
	==========
	result of  中文
	second one 你 <b class="match term0">中文</b>测试<b class="match term0">中文</b> is even more interesting
	==========
	result of  交换机
	干事每月经过下属科室都要亲口交代24口<b class="match term0">交换机</b>等技术性器件的安装工作
	==========
	result of  交换
	咱俩<b class="match term0">交换</b>一下吧
	干事每月经过下属科室都要亲口交代24口<b class="match term0">交换</b>机等技术性器件的安装工作
	==========
	我
	好
	朋友
	是
	李明
	我
	爱
	北京
	天安
	天安门
	ibm
	microsoft
	dream
	intetest
	interest
	me
	lot

## 命令行分词
> 使用示例：`python -m jieba news.txt > cut_result.txt`
> 命令行选项（翻译）：`python -m jieba [options] filename`

固定参数:

	filename 输入文件

可选参数:

	-h, --help 显示此帮助信息并退出
	-d [DELIM], --delimiter [DELIM]
	使用 DELIM 分隔词语，而不是用默认的' / '。
	若不指定 DELIM，则使用一个空格分隔。
	-p [DELIM], --pos [DELIM]
	启用词性标注；如果指定 DELIM，词语和词性之间
	用它分隔，否则用 _ 分隔
	-D DICT, --dict DICT 使用 DICT 代替默认词典
	-u USER_DICT, --user-dict USER_DICT
	使用 USER_DICT 作为附加词典，与默认词典或自定义词典配合使用
	-a, --cut-all 全模式分词（不支持词性标注）
	-n, --no-hmm 不使用隐含马尔可夫模型
	-q, --quiet 不输出载入信息到 STDERR
	-V, --version 显示版本信息并退出
	如果没有指定文件名，则使用标准输入。

## 延迟加载机制
`jieba`采用延迟加载，`import jieba`和`jieba.Tokenizer()`不会立即触发词典的加载，一旦有必要才开始加载词典构建前缀字典。如果你想手工初始`jieba`，也可以手动初始化。

	import jieba
	 
	jieba.initialize() # 手动初始化（可选）

在`0.28`之前的版本是不能指定主词典的路径的，有了延迟加载机制后，你可以改变主词典的路径:

	jieba.set_dictionary('data/dict.txt.big')

例子：

	# encoding=utf-8
	from __future__ import print_function
	import sys
	 
	sys.path.append("../")
	import jieba
	 
	 
	def cuttest(test_sent):
	    result = jieba.cut(test_sent)
	    print("  ".join(result))
	 
	 
	def testcase():
	    cuttest("这是一个伸手不见五指的黑夜。我叫孙悟空，我爱北京，我爱Python和C++。")
	    cuttest("我不喜欢日本和服。")
	    cuttest("雷猴回归人间。")
	    cuttest("工信处女干事每月经过下属科室都要亲口交代24口交换机等技术性器件的安装工作")
	    cuttest("我需要廉租房")
	    cuttest("永和服装饰品有限公司")
	    cuttest("我爱北京天安门")
	    cuttest("abc")
	    cuttest("隐马尔可夫")
	    cuttest("雷猴是个好网站")
	 
	 
	if __name__ == "__main__":
	    testcase()
	    jieba.set_dictionary("foobar.txt")
	    print("================================")
	    testcase()

结果：

	这是  一个  伸手不见五指  的  黑夜  。  我  叫  孙悟空  ，  我  爱  北京  ，  我  爱  Python  和  C++  。
	我  不  喜欢  日本  和服  。
	雷猴  回归  人间  。
	工信处  女干事  每月  经过  下属  科室  都  要  亲口  交代  24  口  交换机  等  技术性  器件  的  安装  工作
	我  需要  廉租房
	永和  服装  饰品  有限公司
	我  爱  北京  天安门
	abc
	隐  马尔可夫
	雷猴  是  个  好  网站

## 其他词典

- 占用内存较小的词典文件:<https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.small>
- 支持繁体分词更好的词典文件:<https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.big>

下载你所需要的词典，然后覆盖`jieba/dict.txt`即可,或者用`jieba.set_dictionary('data/dict.txt.big')`.