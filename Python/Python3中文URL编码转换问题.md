> 原文地址：<https://blog.csdn.net/AnYeZhiYin/article/details/82964725>


	#先引入模块
	from urllib.request import quote
	>>> ff = '摄像头'
	>>> ff = quote(ff)
	>>> ff
	'%E6%91%84%E5%83%8F%E5%A4%B4'
	>>> 
	解码是另一个模块
	from urllib import parse
	>>> aa = '%E6%91%84%E5%83%8F%E5%A4%B4'
	>>> parse.unquote(aa)
	'摄像头'
	>>> 

