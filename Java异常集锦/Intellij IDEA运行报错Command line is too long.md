> 原文地址：<https://blog.csdn.net/ZXJ_1223/article/details/80611089>

# 报错内容

<font color=red>Error running 'ServiceStarter': Command line is too long. Shorten command line for ServiceStarter or also for Application default configuration.</font>

# 解法办法

修改项目下`.idea\workspace.xml`，找到标签`<component name="PropertiesComponent">`， 在标签里加一行  `<property name="dynamic.classpath" value="true" />`。