官方文档地址：<http://tomcat.apache.org/tomcat-7.0-doc/logging.html>
# Tomcat JULI
Tomcat的日志管理功能是借助于`Apache Commons Logging`库来实现的，该库是对当今几个流行的日志框架的精简和封装，从而使得Tomcat日志管理不必依赖于某一个具体的日志框架。

从Tomcat 6.0开始，Tomcat内的`Apache Commons Logging`日志库默认使用`java.util.logging`日志框架实现，如果你想要使用其他的日志框架，只需用对应框架的jar替换掉Tomcat原来的jar即可，日志框架的选择支持以下三种方式：

- 使用系统自带的logging API: java.util.logging
- 使用java servlet规范提供的logging API: javax.servlet.ServletContext.log(..)
- 选择使用其他的日志框架，如log4j

需要注意的是调用`Java Servlets logging API`打印的日志会被`Tomcat`内部日志系统接管，开发者不能设置日志的打印级别：

- 调用`ServletContext.log(String)`或者`GenericServlet.log(String)`打印的日志级别为`INFO`
- 调用`ServletContext.log(String, Throwable)`或者`GenericServlet.log(String, Throwable)`打印的日志级别为`SEVERE`

# 使用java.util.logging（默认）
由于JDK自带的`java.util.logging`实现提供的日志管理能力极为有限，不支持应用级别日志管理。因此，`Tomcat`默认的日志库对`java.util.logging API`进行了重新实现，这些实现被称为`JULI` ，里面包含了一些特有的定制类，其中最重要的是一个自定义的`LogManager`类，它能够区别出运行在Tomcat容器中的多个不同的Web应用以及它们的类加载器，从而可以支持不同的应用使用各自独立的日志配置。

你可以从Tomcat全局和Web应用两个层面对Tomcat默认的JULI进行日志配置：

- 全局配置通常使用`${catalina.base}/conf/logging.properties`文件进行配置,如果该文件未配置或不可读，Tomcat将会使用JRE中的`${java.home}/lib/logging.properties`配置文件；
- 应用配置则是使用`WEB-INF/classes/logging.properties`文件进行配置；

JRE 默认的`logging.properties`仅指定了一个`ConsoleHandler`用于将日志内容打印至控制台。

	############################################################
	#  Default Logging Configuration File
	#
	# You can use a different file by specifying a filename
	# with the java.util.logging.config.file system property.  
	# For example java -Djava.util.logging.config.file=myfile
	############################################################
	 
	############################################################
	#  Global properties
	############################################################
	 
	# "handlers" specifies a comma separated list of log Handler 
	# classes.  These handlers will be installed during VM startup.
	# Note that these classes must be on the system classpath.
	# By default we only configure a ConsoleHandler, which will only
	# show messages at the INFO and above levels.
	handlers= java.util.logging.ConsoleHandler
	 
	# To also add the FileHandler, use the following line instead.
	#handlers= java.util.logging.FileHandler, java.util.logging.ConsoleHandler
	 
	# Default global logging level.
	# This specifies which kinds of events are logged across
	# all loggers.  For any given facility this global level
	# can be overriden by a facility specific level
	# Note that the ConsoleHandler also has a separate level
	# setting to limit messages printed to the console.
	.level= INFO
	 
	############################################################
	# Handler specific properties.
	# Describes specific configuration info for Handlers.
	############################################################
	 
	# default file output is in user's home directory.
	java.util.logging.FileHandler.pattern = %h/java%u.log
	java.util.logging.FileHandler.limit = 50000
	java.util.logging.FileHandler.count = 1
	java.util.logging.FileHandler.formatter = java.util.logging.XMLFormatter
	 
	# Limit the message that are printed on the console to INFO and above.
	java.util.logging.ConsoleHandler.level = INFO
	java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
	 
	# Example to customize the SimpleFormatter output format 
	# to print one-line log message like this:
	#     <level>: <log message> [<date/time>]
	#
	# java.util.logging.SimpleFormatter.format=%4$s: %5$s [%1$tc]%n
	 
	############################################################
	# Facility specific properties.
	# Provides extra control for each logger.
	############################################################
	 
	# For example, set the com.xyz.foo logger to only log SEVERE
	# messages:
	com.xyz.foo.level = SEVERE

Tomcat默认的`conf/logging.properties`指定的处理器除`ConsoleHandler`外还包含了几个文件处理器用来将日志内容输出到文件。

	# Licensed to the Apache Software Foundation (ASF) under one or more
	# contributor license agreements.  See the NOTICE file distributed with
	# this work for additional information regarding copyright ownership.
	# The ASF licenses this file to You under the Apache License, Version 2.0
	# (the "License"); you may not use this file except in compliance with
	# the License.  You may obtain a copy of the License at
	#
	#     http://www.apache.org/licenses/LICENSE-2.0
	#
	# Unless required by applicable law or agreed to in writing, software
	# distributed under the License is distributed on an "AS IS" BASIS,
	# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	# See the License for the specific language governing permissions and
	# limitations under the License.
	 
	handlers = 1catalina.org.apache.juli.FileHandler, 2localhost.org.apache.juli.FileHandler, 3manager.org.apache.juli.FileHandler, 4host-manager.org.apache.juli.FileHandler, java.util.logging.ConsoleHandler
	 
	.handlers = 1catalina.org.apache.juli.FileHandler, java.util.logging.ConsoleHandler
	 
	############################################################
	# Handler specific properties.
	# Describes specific configuration info for Handlers.
	############################################################
	 
	1catalina.org.apache.juli.FileHandler.level = FINE
	1catalina.org.apache.juli.FileHandler.directory = ${catalina.base}/logs
	1catalina.org.apache.juli.FileHandler.prefix = catalina.
	 
	2localhost.org.apache.juli.FileHandler.level = FINE
	2localhost.org.apache.juli.FileHandler.directory = ${catalina.base}/logs
	2localhost.org.apache.juli.FileHandler.prefix = localhost.
	 
	3manager.org.apache.juli.FileHandler.level = FINE
	3manager.org.apache.juli.FileHandler.directory = ${catalina.base}/logs
	3manager.org.apache.juli.FileHandler.prefix = manager.
	 
	4host-manager.org.apache.juli.FileHandler.level = FINE
	4host-manager.org.apache.juli.FileHandler.directory = ${catalina.base}/logs
	4host-manager.org.apache.juli.FileHandler.prefix = host-manager.
	 
	java.util.logging.ConsoleHandler.level = FINE
	java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
	 
	 
	 
	############################################################
	# Facility specific properties.
	# Provides extra control for each logger.
	############################################################
	 
	org.apache.catalina.core.ContainerBase.[Catalina].[localhost].level = INFO
	org.apache.catalina.core.ContainerBase.[Catalina].[localhost].handlers = 2localhost.org.apache.juli.FileHandler
	 
	org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/manager].level = INFO
	org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/manager].handlers = 3manager.org.apache.juli.FileHandler
	 
	org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/host-manager].level = INFO
	org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/host-manager].handlers = 4host-manager.org.apache.juli.FileHandler
	 
	# For example, set the org.apache.catalina.util.LifecycleBase logger to log
	# each component that extends LifecycleBase changing state:
	#org.apache.catalina.util.LifecycleBase.level = FINE
	 
	# To see debug messages in TldLocationsCache, uncomment the following line:
	#org.apache.jasper.compiler.TldLocationsCache.level = FINE

处理器默认的日志级别是`INFO`，可选的日志级别从高到低依次为：`SEVERE` > `WARNING` > `INFO` > `CONFIG` > `FINE` > `FINER` > `FINEST` > `ALL` > `OFF`。

你还可以设置指定包的日志级别，例如打印Tomcat调试级别日志可使用如下配置：

	org.apache.catalina.level=FINEST

JULI的日志配置和JDK中`java.util.logging`的配置极为相似，同时，为了实现更高的日志配置灵活性做了少许的扩展：

- 为了实现能够实例化同一个类的多个处理器，需要在处理器全限定名之前加上一个以数字开头、以`.`结束的前缀，例如：`1catalina.` 。
- 使用`${属性名}`来代表某属性所对应的属性值。
- 使用`loggerName.handlers`属性来为指定名称的`Logger`定义一个处理器集合。
- 使用`.handlers`属性来为根`Logger`定义一个处理器集合。
- 默认情况下，`Logger`如果包含有相关的处理器是不会将日志管理的工作委派给它的父Logger的，可以使用 `loggerName.useParentHandlers=true/false`属性来配置。
- 默认情况下，日志的记录文件是会被永久保存在服务器上的，可以使用`handlerName.maxDays`属性来配置日志文件可保留的最大天数，设置<=0则会永久保存。
- `org.apache.juli.FileHandler`支持使用缓冲输出日志，默认不使用缓冲，可以使用bufferSize属性设置缓冲流大小，设置`0`会使用默认8K，设置负数则不使用缓冲。

# 使用Log4j
如果你想要在你的Web项目中使用`Log4j`对日志进行管理，只需要把`log4j.jar`和`log4j.properties`两个文件分别添加到你Web应用的`WEB-INF/lib`和`WEB-INF/classes`目录下。
如果你想要使用`Log4j`替换`Tomcat`默认的`JULI`，需要执行以下几步操作：

1. 下载与Tomcat版本对应的Log4j实现的`tomcat-juli.jar`和`tomcat-juli-adapters.jar`，注意此`tomcat-juli.jar`与Tomcat自带的JULI包虽然名称相同但实现不同。下载地址：<http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.64/bin/extras/>
2. 下载Log4j.jar，下载地址：<https://logging.apache.org/log4j/2.x/download.html>
3. 将`log4j.jar`和`tomcat-juli-adapters.jar`拷贝到`$CATALINA_HOME/lib`目录， 使用新下载的`tomcat-juli.jar`替换掉Tomcat 自带的`$CATALINA_HOME/bin/tomcat-juli.jar` 。
4. 在`$CATALINA_HOME/lib`目录下创建一个`log4j.properties`日志配置文件。
5. 删除掉`$CATALINA_HOME/conf/`目录下的`logging.properties`文件防止`java.util.logging`生成`0`字节大小的日志文件。
6. 重启Tomcat 。

	log4j.rootLogger = INFO, CATALINA
	 
	# Define all the appenders
	log4j.appender.CATALINA = org.apache.log4j.DailyRollingFileAppender
	log4j.appender.CATALINA.File = ${catalina.base}/logs/catalina
	log4j.appender.CATALINA.Append = true
	log4j.appender.CATALINA.Encoding = UTF-8
	# Roll-over the log once per day
	log4j.appender.CATALINA.DatePattern = '.'yyyy-MM-dd'.log'
	log4j.appender.CATALINA.layout = org.apache.log4j.PatternLayout
	log4j.appender.CATALINA.layout.ConversionPattern = %d [%t] %-5p %c- %m%n
	 
	log4j.appender.LOCALHOST = org.apache.log4j.DailyRollingFileAppender
	log4j.appender.LOCALHOST.File = ${catalina.base}/logs/localhost
	log4j.appender.LOCALHOST.Append = true
	log4j.appender.LOCALHOST.Encoding = UTF-8
	log4j.appender.LOCALHOST.DatePattern = '.'yyyy-MM-dd'.log'
	log4j.appender.LOCALHOST.layout = org.apache.log4j.PatternLayout
	log4j.appender.LOCALHOST.layout.ConversionPattern = %d [%t] %-5p %c- %m%n
	 
	log4j.appender.MANAGER = org.apache.log4j.DailyRollingFileAppender
	log4j.appender.MANAGER.File = ${catalina.base}/logs/manager
	log4j.appender.MANAGER.Append = true
	log4j.appender.MANAGER.Encoding = UTF-8
	log4j.appender.MANAGER.DatePattern = '.'yyyy-MM-dd'.log'
	log4j.appender.MANAGER.layout = org.apache.log4j.PatternLayout
	log4j.appender.MANAGER.layout.ConversionPattern = %d [%t] %-5p %c- %m%n
	 
	log4j.appender.HOST-MANAGER = org.apache.log4j.DailyRollingFileAppender
	log4j.appender.HOST-MANAGER.File = ${catalina.base}/logs/host-manager
	log4j.appender.HOST-MANAGER.Append = true
	log4j.appender.HOST-MANAGER.Encoding = UTF-8
	log4j.appender.HOST-MANAGER.DatePattern = '.'yyyy-MM-dd'.log'
	log4j.appender.HOST-MANAGER.layout = org.apache.log4j.PatternLayout
	log4j.appender.HOST-MANAGER.layout.ConversionPattern = %d [%t] %-5p %c- %m%n
	 
	log4j.appender.CONSOLE = org.apache.log4j.ConsoleAppender
	log4j.appender.CONSOLE.Encoding = UTF-8
	log4j.appender.CONSOLE.layout = org.apache.log4j.PatternLayout
	log4j.appender.CONSOLE.layout.ConversionPattern = %d [%t] %-5p %c- %m%n
	 
	# Configure which loggers log to which appenders
	log4j.logger.org.apache.catalina.core.ContainerBase.[Catalina].[localhost] = INFO, LOCALHOST
	log4j.logger.org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/manager] = INFO, MANAGER
	log4j.logger.org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/host-manager] = INFO, HOST-MANAGER


