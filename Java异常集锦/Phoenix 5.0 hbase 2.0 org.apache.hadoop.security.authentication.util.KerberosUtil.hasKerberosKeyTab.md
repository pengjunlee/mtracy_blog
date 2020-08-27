> 原文地址：<https://www.cnblogs.com/sweetchildomine/p/9690106.html>

	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	    <parent>
	        <artifactId>hadoop</artifactId>
	        <groupId>org.lzw.example</groupId>
	        <version>1.0-SNAPSHOT</version>
	    </parent>
	    <modelVersion>4.0.0</modelVersion>
	    <artifactId>phoenix</artifactId>
	 
	    <dependencies>
	        <!-- https://mvnrepository.com/artifact/org.apache.phoenix/phoenix-core -->
	        <dependency>
	            <groupId>org.apache.phoenix</groupId>
	            <artifactId>phoenix-core</artifactId>
	            <version>5.0.0-HBase-2.0</version>
	        </dependency>
	    </dependencies>
	 
	</project>

异常：<font color=red>java.lang.NoSuchMethodError: org.apache.hadoop.security.authentication.util.KerberosUtil.hasKerberosKeyTab(Ljavax/security/auth/Subject;)Z</font>

加上依赖解决：

	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	    <parent>
	        <artifactId>hadoop</artifactId>
	        <groupId>org.lzw.example</groupId>
	        <version>1.0-SNAPSHOT</version>
	    </parent>
	    <modelVersion>4.0.0</modelVersion>
	 
	    <artifactId>phoenix</artifactId>
	 
	    <dependencies>
	 
	        <!-- https://mvnrepository.com/artifact/org.apache.phoenix/phoenix-core -->
	        <dependency>
	            <groupId>org.apache.phoenix</groupId>
	            <artifactId>phoenix-core</artifactId>
	            <version>5.0.0-HBase-2.0</version>
	        </dependency>
	 
	        <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common -->
	        <dependency>
	            <groupId>org.apache.hadoop</groupId>
	            <artifactId>hadoop-common</artifactId>
	            <version>2.8.4</version>
	        </dependency>
	 
	    </dependencies>
	 
	</project>
 