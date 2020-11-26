> 原文地址：<https://blog.csdn.net/pengjunlee/article/details/92806360>

# 问题描述
centos7系统中通过selenium截图Chrome浏览器图片，发现汉字都为方框，图片如下： 
<div align=center>

![Scrapy](./imgs/29.png "Scrapy示意图")
<div align=left>


# 解决方法 
网上已经有解决方法，就是通过安装字体的方式，网上最多的方式就是安装bitmap字体，我测试后发现无法解决该问题，最终通过安装中文字体总体后解决。

安装过程如下：

下载宋体文件，把下载好的`simsun.ttc`文件放到`/usr/share/fonts/simsun.ttc`目录。

网盘链接：<https://pan.baidu.com/s/1_CXNfm0l5Lq_AtBaMxWpBg>           提取码：**p9oi** 

依次执行如下命令

	mkfontdir
	mkfontscale
	fc-cache -fv

执行下面的命令查看安装的中文语言。

	fc-list :lang=zh
	/usr/share/fonts/simsun.ttc: SimSun,宋体:style=Regular
	/usr/share/fonts/bitmap/fangsongti24.pcf.gz: Fangsong ti:style=Regular
	/usr/share/fonts/simsun.ttc: NSimSun,新宋体:style=Regular
	/usr/share/fonts/bitmap/fangsongti16.pcf.gz: Fangsong ti:style=Regular
 
# 参考文章 

[centos7中Chrome通过selenium截图汉字显示为方框](centos7中Chrome通过selenium截图汉字显示为方框 "https://blog.csdn.net/u011550708/article/details/84587439")

[Centos7安装中文宋体-phantomjs](Centos7安装中文宋体-phantomjs "https://www.aityp.com/centos7%E5%AE%89%E8%A3%85%E4%B8%AD%E6%96%87%E5%AE%8B%E4%BD%93-phantomjs/")