# JS打开窗口的两种方式
## 方式一
使用超链接

	<a href="https://www.csdn.net/" title="CSDN">CSDN</a>

等效于js代码

	window.location.href="https://www.csdn.net/";     //在当前窗口中打开网址

## 方式二

使用超链接

	<a href="https://www.csdn.net/" title="CSDN" target="_blank">CSDN</a>

等效于js代码

	window.open("https://www.csdn.net/");                 //在新的空白窗口中打开网址