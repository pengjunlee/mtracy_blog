# 异常描述
之前一直使用`commons-lang3-3.x.jar`这个jar包里面的`org.apache.commons.lang3.StringEscapeUtils`类来转义特殊字符，但是最近发现使用这个类会出现以下提示： 

	Multiple markers at this line
		- The type StringEscapeUtils is deprecated
		- The method escapeXml11(String) from the type StringEscapeUtils is deprecated

# 解决办法
`StringEscapeUtils`这个类已经过期了，需要引入它的替代类`org.apache.commons.text.StringEscapeUtils`的Maven依赖，如下所示：

	<dependency>
	    <groupId>org.apache.commons</groupId>
	    <artifactId>commons-text</artifactId>
	    <version>1.1</version>
	</dependency>