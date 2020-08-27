# 异常描述
Maven项目缺少Jar包--`jdk.toolss:jar:1.8`。

	Error message:Missing artifact jdk.tools:jdk.tools:jar:1.8   [Maven Dependency Problem]
	The container 'Maven Dependencies' references non existing library 'D:\.m2\repository\jdk\tools\jdk.tools\1.8\jdk.tools-1.8.jar'


# 解决办法
需要引入Maven依赖，如下所示：

	<dependency>
	    <groupId>jdk.tools</groupId>
	    <artifactId>jdk.tools</artifactId>
	    <version>1.8</version>
	    <scope>system</scope>
	    <systemPath>${JAVA_HOME}/lib/tools.jar</systemPath>
	</dependency>