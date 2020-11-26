> 原文链接：<https://www.cnblogs.com/zsg88/p/9473843.html>

直接用`java -jar xxx.jar`，当退出或关闭`shell`时，程序就会停止掉。以下方法可让`jar`运行后一直在后台运行。

# 1. 

	java -jar xxx.jar &

<font color=red>说明</font>： 在末尾加入`&`符号

# 2.

1. 执行 java -jar xxx.jar 后
2. ctrl+z 退出到控制台，执行 bg
3. exit

完成以上3步，退出SHELL后，jar服务一直在后台运行。

# 3.

	nohup java -jar xxxx.jar & 

将`java -jar xxxx.jar`加入`nohup`和`&`中间，也可以实现。