> 原文地址：<https://blog.csdn.net/vili_sky/article/details/79537088>

# 异常描述
今天做微信开发时，需要给微信服务器回复json格式的数据，所以在maven项目中添加`net.sf.json`的依赖，官方查到的依赖是： 

	<!-- https://mvnrepository.com/artifact/net.sf.json-lib/json-lib -->
	<dependency>
	    <groupId>net.sf.json-lib</groupId>
	    <artifactId>json-lib</artifactId>
	    <version>2.4</version>
	</dependency>

但是在`pom.xml`文件中加入这个依赖之后，还是无法`import net.sf.json.JSONObject;`，`import`时候已然报错，说明`json-lib`的jar包没有引入成功。 

之后又换了几个版本试了下，还是不能**import**，一开始我还以为是这个依赖下载的jar包名称是`json-lib-2.4-jdk15`， 说明是`jdk1.5`版本的，我用的JDK8，可能有哪些不兼容什么的，但是按道理都是向下兼容的，郁闷了挺久的。 

# 解决办法
慢慢排查， 查询多方资料之后，发现如果在引入依赖的时候指定jdk的版本号，就能够下载到，如下

	<dependency>
	    <groupId>net.sf.json-lib</groupId>
	    <artifactId>json-lib</artifactId>
	    <version>2.4</version>
	    <classifier>jdk15</classifier>
	</dependency>

而且只能指定15，反正我试其他的没有用，因为jar包就只有叫`json-lib-2.4-jdk15`的。由此，解决了。