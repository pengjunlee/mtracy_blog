# 异常描述
<font color=red>web.xml is missing and `<failOnMissingWebXml>` is set to true.</font>

# 解决办法
如果你的工程是web项目，可按如下步骤进行操作。

1. ——>右击项目
2. ——>Java EE Tools
3. ——>Generate Deployment Descriptor Stub

然后，系统会在src/main/webapp/WEB_INF文件加下创建web.xml文件。错误解决！

如果你的工程不是web项目，那么还有另外一种解决方案，就是在pom文件中配置一下`failOnMissingWebXml`。具体配置如下：

	<build>
	  <plugins>
	   <plugin>
	    <groupId>org.apache.maven.plugins</groupId>
	    <artifactId>maven-war-plugin</artifactId>
	    <version>2.6</version>
	    <configuration>
	     <failOnMissingWebXml>false</failOnMissingWebXml>
	    </configuration>
	   </plugin>
	  </plugins>
	 </build>
 