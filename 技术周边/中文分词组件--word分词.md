`word`分词是一个Java实现的分布式的中文分词组件，提供了多种基于词典的分词算法，并利用`ngram`模型来消除歧义。能准确识别`英文`、`数字`，以及`日期`、`时间`等`数量词`，能识别`人名`、`地名`、`组织机构名`等`未登录词`。能通过自定义配置文件来改变组件行为，能自定义用户词库、自动检测词库变化、支持大规模分布式环境，能灵活指定多种分词算法，能使用refine功能灵活控制分词结果，还能使用`词性标注`、`同义标注`、`反义标注`、`拼音标注`等功能。同时还无缝和`Lucene`、`Solr`、`ElasticSearch`、`Luke`集成。

> <font color=red>注意</font>：`word1.3`需要`JDK1.8`

# API在线文档
[word 1.0 API](http://apdplat.org/word/apidocs/1.0/ "word 1.0 API")

[word 1.1 API](http://apdplat.org/word/apidocs/1.1/ "word 1.1 API")

[word 1.2 API](http://apdplat.org/word/apidocs/1.2/ "word 1.2 API")

# 编译好的jar包下载（含依赖)
> 链接: <https://pan.baidu.com/s/1mnGQqx_5Yqv_KxS9HJCTcA>   
> 提取码: `essu`

# Maven依赖
在`pom.xml`中指定dependency，可用版本有`1.0`、`1.1`、`1.2`、`1.3`。

	<dependencies>
	    <dependency>
	        <groupId>org.apdplat</groupId>
	        <artifactId>word</artifactId>
	        <version>1.3</version>
	    </dependency>
	</dependencies>

# 分词使用方法
## 快速体验

运行项目根目录下的脚本`demo-word.bat`可以快速体验分词效果。
> 用法: command [text] [input] [output]  
> command的可选值为：demo、text、file  

	demo
	text 杨尚川是APDPlat应用级产品开发平台的作者
	file d:/text.txt d:/word.txt
	exit

## 对文本进行分词

	// 移除停用词
	List<Word> words = WordSegmenter.seg("杨尚川是APDPlat应用级产品开发平台的作者");
	// 保留停用词
	List<Word> words = WordSegmenter.segWithStopWords("杨尚川是APDPlat应用级产品开发平台的作者");
	System.out.println(words);
 
输出：

	移除停用词：[杨尚川, apdplat, 应用级, 产品, 开发平台, 作者]
	保留停用词：[杨尚川, 是, apdplat, 应用级, 产品, 开发平台, 的, 作者]

## 对文件进行分词

	String input = "d:/text.txt";
	String output = "d:/word.txt";
	// 移除停用词
	WordSegmenter.seg(new File(input), new File(output));
	// 保留停用词
	WordSegmenter.segWithStopWords(new File(input), new File(output));

## 自定义配置文件

- 默认配置文件为类路径下的`word.conf`，打包在`word-x.x.jar`中；
- 自定义配置文件为类路径下的`word.local.conf`，需要用户自己提供；
- 如果自定义配置和默认配置相同，自定义配置会覆盖默认配置；
- 配置文件编码为`UTF-8`；

## 自定义用户词库

- 自定义用户词库为一个或多个文件夹或文件，可以使用绝对路径或相对路径；
- 用户词库由多个词典文件组成，文件编码为`UTF-8`；
- 词典文件的格式为文本文件，一行代表一个词；
- 可以通过系统属性或配置文件的方式来指定路径，多个路径之间用逗号分隔开；
- 类路径下的词典文件，需要在相对路径前加入前缀`classpath:`；
- 如未指定，则默认使用类路径下的`dic.txt`词典文件；
 
指定方式有三种：

    // 指定方式一，编程指定（高优先级）：
    WordConfTools.set("dic.path", "classpath:dic.txt，d:/custom_dic");
    DictionaryFactory.reload(); //更改词典路径之后，重新加载词典
    // 指定方式二，Java虚拟机启动参数（中优先级）：
    java -Ddic.path=classpath:dic.txt，d:/custom_dic
    // 指定方式三，配置文件指定（低优先级）：
    // 使用类路径下的文件word.local.conf来指定配置信息
    dic.path=classpath:dic.txt，d:/custom_dic
 

## 自定义停用词词库

使用方式和自定义用户词库类似，配置项为：

	stopwords.path=classpath:stopwords.txt，d:/custom_stopwords_dic

## 自动检测词库变化

可以自动检测自定义用户词库和自定义停用词词库的变化，包含类路径下的文件和文件夹、非类路径下的绝对路径和相对路径
如：

	classpath:dic.txt，classpath:custom_dic_dir,
	d:/dic_more.txt，d:/DIC_DIR，D:/DIC2_DIR，my_dic_dir，my_dic_file.txt
	 
	classpath:stopwords.txt，classpath:custom_stopwords_dic_dir，
	d:/stopwords_more.txt，d:/STOPWORDS_DIR，d:/STOPWORDS2_DIR，stopwords_dir，remove.txt

## 显式指定分词算法

对文本进行分词时，可显式指定特定的分词算法，如：

	WordSegmenter.seg("APDPlat应用级产品开发平台", SegmentationAlgorithm.BidirectionalMaximumMatching);
 
`SegmentationAlgorithm`的可选类型为：

- 正向最大匹配算法：MaximumMatching
- 逆向最大匹配算法：ReverseMaximumMatching
- 正向最小匹配算法：MinimumMatching
- 逆向最小匹配算法：ReverseMinimumMatching
- 双向最大匹配算法：BidirectionalMaximumMatching
- 双向最小匹配算法：BidirectionalMinimumMatching
- 双向最大最小匹配算法：BidirectionalMaximumMinimumMatching
- 全切分算法：FullSegmentation
- 最少分词算法：MinimalWordCount
- 最大Ngram分值算法：MaxNgramScore

## 分词效果评估

运行项目根目录下的脚本`evaluation.bat`可以对分词效果进行评估，评估采用的测试文本有2533709行，共28374490个字符，评估结果位于target/evaluation目录下：

- `corpus-text.txt`为分好词的人工标注文本，词之间以空格分隔
- `test-text.txt`为测试文本，是把`corpus-text.txt`以标点符号分隔为多行的结果
- `standard-text.txt`为测试文本对应的人工标注文本，作为分词是否正确的标准
- `result-text-***.txt`，`***`为各种分词算法名称，这是word分词结果
- `perfect-result-***.txt`，`***`为各种分词算法名称，这是分词结果和人工标注标准完全一致的文本
- `wrong-result-***.txt`，`***`为各种分词算法名称，这是分词结果和人工标注标准不一致的文本

## 分布式中文分词器

1. 在自定义配置文件`word.conf`或`word.local.conf`中指定所有的配置项`*.path`使用HTTP资源，同时指定配置项`redis.*`
2. 配置并启动提供HTTP资源的web服务器，将项目：`https://github.com/ysc/word_web`部署到tomcat
3. 配置并启动redis服务器

## 词性标注（1.3才有这个功能）

将分词结果作为输入参数，调用`PartOfSpeechTagging`类的`process`方法，词性保存在`Word`类的`partOfSpeech`字段中，如下所示：

	List<Word> words = WordSegmenter.segWithStopWords("我爱中国");
	System.out.println("未标注词性："+words);
	//词性标注
	PartOfSpeechTagging.process(words);
	System.out.println("标注词性："+words);

输出内容：

	未标注词性：[我, 爱, 中国]
	标注词性：[我/r, 爱/v, 中国/ns]

## refine

我们看一个切分例子：

	List<Word> words = WordSegmenter.segWithStopWords("我国工人阶级和广大劳动群众要更加紧密地团结在党中央周围");
	System.out.println(words);

结果如下：

	[我国, 工人阶级, 和, 广大, 劳动群众, 要, 更加, 紧密, 地, 团结, 在, 党中央, 周围]

假如我们想要的切分结果是：

	[我国, 工人, 阶级, 和, 广大, 劳动, 群众, 要, 更加, 紧密, 地, 团结, 在, 党中央, 周围]

也就是要把`工人阶级`细分为`工人` `阶级`，把`劳动群众`细分为`劳动` `群众`，那么我们该怎么办呢？

我们可以通过在`word.refine.path`配置项指定的文件`classpath:word_refine.txt`中增加以下内容：

	工人阶级=工人 阶级
	劳动群众=劳动 群众

然后，我们对分词结果进行refine：

	words = WordRefiner.refine(words);
	System.out.println(words);

这样，就能达到我们想要的效果：

	[我国, 工人, 阶级, 和, 广大, 劳动, 群众, 要, 更加, 紧密, 地, 团结, 在, 党中央, 周围]
 
我们再看一个切分例子：

	List<Word> words = WordSegmenter.segWithStopWords("在实现“两个一百年”奋斗目标的伟大征程上再创新的业绩");
	System.out.println(words);

结果如下：

	[在, 实现, 两个, 一百年, 奋斗目标, 的, 伟大, 征程, 上, 再创, 新的, 业绩]

假如我们想要的切分结果是：

	[在, 实现, 两个一百年, 奋斗目标, 的, 伟大征程, 上, 再创, 新的, 业绩]

也就是要把`两个` `一百年`合并为`两个一百年`，把`伟大` `征程`合并为“伟大征程”，那么我们该怎么办呢？

我们可以通过在`word.refine.path`配置项指定的文件`classpath:word_refine.txt`中增加以下内容：

	两个 一百年=两个一百年
	伟大 征程=伟大征程

然后，我们对分词结果进行refine：

	words = WordRefiner.refine(words);
	System.out.println(words);

这样，就能达到我们想要的效果：

	[在, 实现, 两个一百年, 奋斗目标, 的, 伟大征程, 上, 再创, 新的, 业绩]

## 同义标注

	List<Word> words = WordSegmenter.segWithStopWords("楚离陌千方百计为无情找回记忆");
	System.out.println(words);

结果如下：

	[楚离陌, 千方百计, 为, 无情, 找回, 记忆]

做同义标注：

	SynonymTagging.process(words);
	System.out.println(words);

结果如下：

	[楚离陌, 千方百计[久有存心, 化尽心血, 想方设法, 费尽心机], 为, 无情, 找回, 记忆[影象]]

如果启用间接同义词：

	SynonymTagging.process(words, false);
	System.out.println(words);

结果如下：

	[楚离陌, 千方百计[久有存心, 化尽心血, 想方设法, 费尽心机], 为, 无情, 找回, 记忆[影像, 影象]]
<br/>

	List<Word> words = WordSegmenter.segWithStopWords("手劲大的老人往往更长寿");
	System.out.println(words);

结果如下：

	[手劲, 大, 的, 老人, 往往, 更, 长寿]

做同义标注：

	SynonymTagging.process(words);
	System.out.println(words);

结果如下：

	[手劲, 大, 的, 老人[白叟], 往往[常常, 每每, 经常], 更, 长寿[长命, 龟龄]]

如果启用间接同义词：

	SynonymTagging.process(words, false);
	System.out.println(words);

结果如下：

	[手劲, 大, 的, 老人[白叟], 往往[一样平常, 一般, 凡是, 寻常, 常常, 常日, 平凡, 平居, 平常, 平日, 平时, 往常, 日常, 日常平凡, 时常, 普通, 每每, 泛泛, 素日, 经常, 通俗, 通常], 更, 长寿[长命, 龟龄]]
 
以词`千方百计`为例：

可以通过`Word`的`getSynonym()`方法获取同义词如：

	System.out.println(word.getSynonym());

结果如下：

	[久有存心, 化尽心血, 想方设法, 费尽心机]

> <font color=red>注意</font>：如果没有同义词，则`getSynonym()`返回空集合：`Collections.emptyList()`
 
间接同义词和直接同义词的区别如下：
假设，A和B是同义词，A和C是同义词，B和D是同义词，C和E是同义词，则：

- 对于A来说，A B C是直接同义词
- 对于B来说，A B D是直接同义词
- 对于C来说，A C E是直接同义词
- 对于A B C来说，A B C D E是间接同义词

## 反义标注
	
	List<Word> words = WordSegmenter.segWithStopWords("5月初有哪些电影值得观看");
	System.out.println(words);

结果如下：

	[5, 月初, 有, 哪些, 电影, 值得, 观看]

做反义标注：

	AntonymTagging.process(words);
	System.out.println(words);

结果如下：

	[5, 月初[月底, 月末, 月终], 有, 哪些, 电影, 值得, 观看]
<br/>

	List<Word> words = WordSegmenter.segWithStopWords("由于工作不到位、服务不完善导致顾客在用餐时发生不愉快的事情,餐厅方面应该向顾客作出真诚的道歉,而不是敷衍了事。");
	System.out.println(words);

结果如下：

	[由于, 工作, 不到位, 服务, 不完善, 导致, 顾客, 在, 用餐, 时, 发生, 不愉快, 的, 事情, 餐厅, 方面, 应该, 向, 顾客, 作出, 真诚, 的, 道歉, 而不是, 敷衍了事]

做反义标注：

	AntonymTagging.process(words);
	System.out.println(words);

结果如下：

	[由于, 工作, 不到位, 服务, 不完善, 导致, 顾客, 在, 用餐, 时, 发生, 不愉快, 的, 事情, 餐厅, 方面, 应该, 向, 顾客, 作出, 真诚[糊弄, 虚伪, 虚假, 险诈], 的, 道歉, 而不是, 敷衍了事[一丝不苟, 兢兢业业, 尽心竭力, 竭尽全力, 精益求精, 诚心诚意]]
 
以词`月初`为例，可以通过`Word`的`getAntonym()`方法获取反义词如：

	System.out.println(word.getAntonym());

结果如下：

	[月底, 月末, 月终]

> <font color=red>注意</font>：如果没有反义词，`getAntonym()`返回空集合：`Collections.emptyList()`

## 拼音标注

	List<Word> words = WordSegmenter.segWithStopWords("《速度与激情7》的中国内地票房自4月12日上映以来，在短短两周内突破20亿人民币");
	System.out.println(words);

结果如下：

	[速度, 与, 激情, 7, 的, 中国, 内地, 票房, 自, 4月, 12日, 上映, 以来, 在, 短短, 两周, 内, 突破, 20亿, 人民币]

执行拼音标注：

	PinyinTagging.process(words);
	System.out.println(words);

结果如下：

	[速度 sd sudu, 与 y yu, 激情 jq jiqing, 7, 的 d de, 中国 zg zhongguo, 内地 nd neidi, 票房 pf piaofang, 自 z zi, 4月, 12日, 上映 sy shangying, 以来 yl yilai, 在 z zai, 短短 dd duanduan, 两周 lz liangzhou, 内 n nei, 突破 tp tupo, 20亿, 人民币 rmb renminbi]
 
以词`速度`为例：

- 可以通过Word的`getFullPinYin()`方法获取完整拼音如：`sudu`
- 可以通过Word的`getAcronymPinYin()`方法获取首字母缩略拼音如：`sd`

## Lucene插件：

### 构造一个word分析器ChineseWordAnalyzer

	Analyzer analyzer = new ChineseWordAnalyzer();

如果需要使用特定的分词算法，可通过构造函数来指定：

	Analyzer analyzer = new ChineseWordAnalyzer(SegmentationAlgorithm.FullSegmentation);

如不指定，默认使用双向最大匹配算法：`SegmentationAlgorithm.BidirectionalMaximumMatching`，可用的分词算法参见枚举类：`SegmentationAlgorithm`
 
### 利用word分析器切分文本

	TokenStream tokenStream = analyzer.tokenStream("text", "杨尚川是APDPlat应用级产品开发平台的作者");
	//准备消费
	tokenStream.reset();
	//开始消费
	while(tokenStream.incrementToken()){
	    //词
	    CharTermAttribute charTermAttribute = tokenStream.getAttribute(CharTermAttribute.class);
	    //词在文本中的起始位置
	    OffsetAttribute offsetAttribute = tokenStream.getAttribute(OffsetAttribute.class);
	    //第几个词
	    PositionIncrementAttribute positionIncrementAttribute = tokenStream.getAttribute(PositionIncrementAttribute.class);
	    //词性
	    PartOfSpeechAttribute partOfSpeechAttribute = tokenStream.getAttribute(PartOfSpeechAttribute.class);
	    //首字母缩略拼音
	    AcronymPinyinAttribute acronymPinyinAttribute = tokenStream.getAttribute(AcronymPinyinAttribute.class);
	    //完整拼音
	    FullPinyinAttribute fullPinyinAttribute = tokenStream.getAttribute(FullPinyinAttribute.class);
	    //同义词
	    SynonymAttribute synonymAttribute = tokenStream.getAttribute(SynonymAttribute.class);
	    //反义词
	    AntonymAttribute antonymAttribute = tokenStream.getAttribute(AntonymAttribute.class);
	 
	    LOGGER.info(charTermAttribute.toString()+" ("+offsetAttribute.startOffset()+" - "+offsetAttribute.endOffset()+") "+positionIncrementAttribute.getPositionIncrement());
	    LOGGER.info("PartOfSpeech:"+partOfSpeechAttribute.toString());
	    LOGGER.info("AcronymPinyin:"+acronymPinyinAttribute.toString());
	    LOGGER.info("FullPinyin:"+fullPinyinAttribute.toString());
	    LOGGER.info("Synonym:"+synonymAttribute.toString());
	    LOGGER.info("Antonym:"+antonymAttribute.toString());
	}
	//消费完毕
	tokenStream.close();
 
### 利用word分析器建立Lucene索引

	Directory directory = new RAMDirectory();
	IndexWriterConfig config = new IndexWriterConfig(analyzer);
	IndexWriter indexWriter = new IndexWriter(directory, config);
 
### 利用word分析器查询Lucene索引

	QueryParser queryParser = new QueryParser("text", analyzer);
	Query query = queryParser.parse("text:杨尚川");
	TopDocs docs = indexSearcher.search(query, Integer.MAX_VALUE);

### Solr插件：

#### 下载word-1.3.jar
下载地址：<http://search.maven.org/remotecontent?filepath=org/apdplat/word/1.3/word-1.3.jar>
 
#### 创建目录`solr-5.1.0/example/solr/lib`，将`word-1.3.jar`复制到`lib`目录
 
#### 配置schema指定分词器
将`solr-5.1.0/example/solr/collection1/conf/schema.xml`文件中所有的`<tokenizer class="solr.WhitespaceTokenizerFactory"/>`和`<tokenizer class="solr.StandardTokenizerFactory"/>`全部替换为`<tokenizer class="org.apdplat.word.solr.ChineseWordTokenizerFactory"/>`，并移除所有的`filter`标签
 
#### 如果需要使用特定的分词算法：
	<tokenizer class="org.apdplat.word.solr.ChineseWordTokenizerFactory" segAlgorithm="ReverseMinimumMatching"/>

segAlgorithm可选值有：  
- 正向最大匹配算法：MaximumMatching
- 逆向最大匹配算法：ReverseMaximumMatching
- 正向最小匹配算法：MinimumMatching
- 逆向最小匹配算法：ReverseMinimumMatching
- 双向最大匹配算法：BidirectionalMaximumMatching
- 双向最小匹配算法：BidirectionalMinimumMatching
- 双向最大最小匹配算法：BidirectionalMaximumMinimumMatching
- 全切分算法：FullSegmentation
- 最少分词算法：MinimalWordCount
- 最大Ngram分值算法：MaxNgramScore

如不指定，默认使用双向最大匹配算法：`BidirectionalMaximumMatching`
 
#### 如果需要指定特定的配置文件：
	<tokenizer class="org.apdplat.word.solr.ChineseWordTokenizerFactory" segAlgorithm="ReverseMinimumMatching" conf="solr-5.1.0/example/solr/nutch/conf/word.local.conf"/>

`word.local.conf`文件中可配置的内容见`word-1.3.jar`中的`word.conf`文件，如不指定，使用默认配置文件，位于`word-1.3.jar`中的`word.conf`文件

## ElasticSearch插件：

**1、 打开命令行并切换到`elasticsearch`的bin目录**
	
	cd elasticsearch-1.5.1/bin
 
**2、 运行plugin脚本安装word分词插件：**

	./plugin -u http://apdplat.org/word/archive/v1.2.zip -i word

**3、 修改文件`elasticsearch-1.5.1/config/elasticsearch.yml`，新增如下配置：**

	index.analysis.analyzer.default.type : "word"
	index.analysis.tokenizer.default.type : "word"
 
**4、 启动ElasticSearch测试效果**，在Chrome浏览器中访问：<http://localhost:9200/_analyze?analyzer=word&text=杨尚川是APDPlat应用级产品开发平台的作者>
 
**5、 自定义配置**，修改配置文件`elasticsearch-1.5.1/plugins/word/word.local.conf`
 
**6、 指定分词算法**，修改文件`elasticsearch-1.5.1/config/elasticsearch.yml`，新增如下配置：

	index.analysis.analyzer.default.segAlgorithm : "ReverseMinimumMatching"
	index.analysis.tokenizer.default.segAlgorithm : "ReverseMinimumMatching"
 
这里segAlgorithm可指定的值有：

- 正向最大匹配算法：MaximumMatching
- 逆向最大匹配算法：ReverseMaximumMatching
- 正向最小匹配算法：MinimumMatching
- 逆向最小匹配算法：ReverseMinimumMatching
- 双向最大匹配算法：BidirectionalMaximumMatching
- 双向最小匹配算法：BidirectionalMinimumMatching
- 双向最大最小匹配算法：BidirectionalMaximumMinimumMatching
- 全切分算法：FullSegmentation
- 最少分词算法：MinimalWordCount
- 最大Ngram分值算法：MaxNgramScore

如不指定，默认使用双向最大匹配算法：`BidirectionalMaximumMatching`

## Luke插件：

1、 下载`http://luke.googlecode.com/files/lukeall-4.0.0-ALPHA.jar`（国内不能访问）
 
2、 下载并解压Java中文分词组件`word-1.0-bin.zip`：`http://pan.baidu.com/s/1dDziDFz`
 
3、 将解压后的Java中文分词组件`word-1.0-bin/word-1.0`文件夹里面的4个jar包解压到当前文件夹
用压缩解压工具如winrar打开`lukeall-4.0.0-ALPHA.jar`，将当前文件夹里面除了`META-INF`文件夹、`.jar`、
`.bat`、`.html`、`word.local.conf`文件外的其他所有文件拖到`lukeall-4.0.0-ALPHA.jar`里面
 
4、 执行命令`java -jar lukeall-4.0.0-ALPHA.jar`启动luke，在Search选项卡的Analysis里面
就可以选择`org.apdplat.word.lucene.ChineseWordAnalyzer`分词器了
 
5、 在Plugins选项卡的`Available analyzers found on the current classpath`里面也可以选择 
`org.apdplat.word.lucene.ChineseWordAnalyzer`分词器
 
> <font color=red>注意</font>：如果你要自己集成word分词器的其他版本，在项目根目录下运行`mvn install`编译项目，然后运行命令`mvn dependency:copy-dependencies`复制依赖的jar包，接着在`target/dependency/`目录下就会有所有的依赖jar包。其中`target/dependency/slf4j-api-1.6.4.jar`是word分词器使用的日志框架，
`target/dependency/logback-classic-0.9.28.jar`和
`target/dependency/logback-core-0.9.28.jar`是word分词器推荐使用的日志实现，日志实现的配置文件
路径位于`target/classes/logback.xml`，`target/word-1.3.jar`是word分词器的主jar包，如果需要
自定义词典，则需要修改分词器配置文件`target/classes/word.conf`。

- 已经集成好的Luke插件下载（适用于lucene4.0.0） ：`lukeall-4.0.0-ALPHA-with-word-1.0.jar`
- 已经集成好的Luke插件下载（适用于lucene4.10.3）：`lukeall-4.10.3-with-word-1.2.jar`

## 词向量：

从大规模语料中统计一个词的上下文相关词，并用这些上下文相关词组成的向量来表达这个词。通过计算词向量的相似性，即可得到词的相似性。相似性的假设是建立在如果两个词的上下文相关词越相似，那么这两个词就越相似这个前提下的。
 
通过运行项目根目录下的脚本`demo-word-vector-corpus.bat`来体验word项目自带语料库的效果。
 
如果有自己的文本内容，可以使用脚本`demo-word-vector-file.bat`来对文本分词、建立词向量、计算相似性

# 分词算法效果评估

	1、word分词 最大Ngram分值算法：
	分词速度：397.73047 字符/毫秒
	行数完美率：59.93%  行数错误率：40.06%  总的行数：2533709  完美行数：1518525  错误行数：1015184
	字数完美率：51.56% 字数错误率：48.43% 总的字数：28374490 完美字数：14632098 错误字数：13742392
	 
	2、word分词 全切分算法：
	分词速度：67.032585 字符/毫秒
	行数完美率：57.2%  行数错误率：42.79%  总的行数：2533709  完美行数：1449288  错误行数：1084421
	字数完美率：47.95% 字数错误率：52.04% 总的字数：28374490 完美字数：13605742 错误字数：14768748
	 
	3、word分词 双向最大最小匹配算法：
	分词速度：367.99805 字符/毫秒
	行数完美率：53.06%  行数错误率：46.93%  总的行数：2533709  完美行数：1344624  错误行数：1189085
	字数完美率：43.07% 字数错误率：56.92% 总的字数：28374490 完美字数：12221610 错误字数：16152880
	 
	4、word分词 最少分词算法：
	分词速度：364.40622 字符/毫秒
	行数完美率：47.75%  行数错误率：52.24%  总的行数：2533709  完美行数：1209976  错误行数：1323733
	字数完美率：37.59% 字数错误率：62.4% 总的字数：28374490 完美字数：10666443 错误字数：17708047
	 
	5、word分词 双向最小匹配算法：
	分词速度：657.13635 字符/毫秒
	行数完美率：46.34%  行数错误率：53.65%  总的行数：2533709  完美行数：1174276  错误行数：1359433
	字数完美率：36.07% 字数错误率：63.92% 总的字数：28374490 完美字数：10236574 错误字数：18137916
	 
	6、word分词 双向最大匹配算法：
	分词速度：539.0905 字符/毫秒
	行数完美率：46.18%  行数错误率：53.81%  总的行数：2533709  完美行数：1170075  错误行数：1363634
	字数完美率：35.65% 字数错误率：64.34% 总的字数：28374490 完美字数：10117122 错误字数：18257368
	 
	7、word分词 正向最大匹配算法：
	分词速度：662.2127 字符/毫秒
	行数完美率：41.88%  行数错误率：58.11%  总的行数：2533709  完美行数：1061189  错误行数：1472520
	字数完美率：31.35% 字数错误率：68.64% 总的字数：28374490 完美字数：8896173 错误字数：19478317
	 
	8、word分词 逆向最大匹配算法：
	分词速度：1082.0459 字符/毫秒
	行数完美率：41.69%  行数错误率：58.3%  总的行数：2533709  完美行数：1056515  错误行数：1477194
	字数完美率：30.98% 字数错误率：69.01% 总的字数：28374490 完美字数：8792532 错误字数：19581958
	 
	9、word分词 逆向最小匹配算法：
	分词速度：1906.6315 字符/毫秒
	行数完美率：41.42%  行数错误率：58.57%  总的行数：2533709  完美行数：1049673  错误行数：1484036
	字数完美率：31.34% 字数错误率：68.65% 总的字数：28374490 完美字数：8893622 错误字数：19480868
	 
	10、word分词 正向最小匹配算法：
	分词速度：1839.1554 字符/毫秒
	行数完美率：36.7%  行数错误率：63.29%  总的行数：2533709  完美行数：930069  错误行数：1603640
	字数完美率：26.72% 字数错误率：73.27% 总的字数：28374490 完美字数：7583741 错误字数：20790749

# 相关文章
[中文分词算法 之 基于词典的正向最大匹配算法](http://yangshangchuan.iteye.com/blog/2031813 "中文分词算法 之 基于词典的正向最大匹配算法")  
[中文分词算法 之 基于词典的逆向最大匹配算法](http://yangshangchuan.iteye.com/blog/2033843 "中文分词算法 之 基于词典的逆向最大匹配算法")  
[中文分词算法 之 词典机制性能优化与测试](http://yangshangchuan.iteye.com/blog/2035007 "中文分词算法 之 词典机制性能优化与测试")  
[中文分词算法 之 基于词典的正向最小匹配算法](http://yangshangchuan.iteye.com/blog/2040423 "中文分词算法 之 基于词典的正向最小匹配算法")  
[中文分词算法 之 基于词典的逆向最小匹配算法](http://yangshangchuan.iteye.com/blog/2040431 "中文分词算法 之 基于词典的逆向最小匹配算法")  
[一种利用ngram模型来消除歧义的中文分词方法](http://my.oschina.net/apdplat/blog/411112 "一种利用ngram模型来消除歧义的中文分词方法")  
[一种基于词性序列的人名识别方法](http://my.oschina.net/apdplat/blog/411032 "一种基于词性序列的人名识别方法")  
[中文分词算法 之 基于词典的全切分算法](http://my.oschina.net/apdplat/blog/412785 "中文分词算法 之 基于词典的全切分算法")  
[9大Java开源中文分词器的使用方法和分词效果对比](http://my.oschina.net/apdplat/blog/412921 "9大Java开源中文分词器的使用方法和分词效果对比")  
[中文分词之11946组同义词](http://my.oschina.net/apdplat/blog/408779 "中文分词之11946组同义词")  
[中文分词之9271组反义词](http://my.oschina.net/apdplat/blog/411301 "中文分词之9271组反义词")  
[如何利用多核提升分词速度](http://my.oschina.net/apdplat/blog/414076 "如何利用多核提升分词速度")  
