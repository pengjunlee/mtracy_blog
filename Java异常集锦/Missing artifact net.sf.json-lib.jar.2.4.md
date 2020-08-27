# 异常描述
Maven项目缺少Jar包--`json-lib:jar:2.4`。

	Error message:Missing artifact net.sf.json-lib:json-lib:jar:2.4


# 解决办法
需要引入Maven依赖，如下所示：

	<!-- https://mvnrepository.com/artifact/net.sf.json-lib/json-lib -->
	<dependency>
		<groupId>net.sf.json-lib</groupId>
		<artifactId>json-lib</artifactId>
		<version>2.4</version>
		<classifier>jdk15</classifier>
	</dependency>