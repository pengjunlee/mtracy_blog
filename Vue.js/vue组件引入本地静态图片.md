> 原文链接：<https://my.oschina.net/liyoungs/blog/3085118>

vue2组件引入本地图片的两种方式：

**1，使用@引入：**

这是在组件内直接引用和普通的html方法一样，代码如下

	<img src="@/assets/test.png" alt="test.png">

**2，使用vue的方法引入：**

这是典型的vue思想，使用数据来操纵dom； 首先在组件内使用 `import ... from` 引入

	import imgUrl from '../assets/test.png';

然后在data里面声明

	data: function () {
	            return {
	                imgSrc: imgUrl
	            }
	        }

最后绑定数据

	<img :src="imgSrc" alt="imgSrc">