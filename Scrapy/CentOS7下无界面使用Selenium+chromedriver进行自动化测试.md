# 安装python与pip
python及pip安装请移步：[Linux下使用源码包安装Python](Linux下使用源码包安装Python "https://blog.csdn.net/pengjunlee/article/details/89100730")

# 安装selenium
你可以使用`pip`命令来安装`Selenium`：

	pip install selenium

 安装过程类似下面这样。

	[root@localhost src]# pip install selenium
	Collecting selenium
	  Downloading https://files.pythonhosted.org/packages/80/d6/4294f0b4bce4de0abf13e17190289f9d0613b0a44e5dd6a7f5ca98459853/selenium-3.141.0-py2.py3-none-any.whl (904kB)
	    100% |████████████████████████████████| 911kB 12kB/s 
	Requirement already satisfied: urllib3 in /usr/local/python3/lib/python3.7/site-packages (from selenium) (1.24.3)
	Installing collected packages: selenium
	Successfully installed selenium-3.141.0
	You are using pip version 19.0.3, however version 19.1.1 is available.
	You should consider upgrading via the 'pip install --upgrade pip' command.

或者，使用`easy_install`来安装`Selenium`：

	easy_install selenium

# 安装chrome
官方文档：<https://intoli.com/blog/installing-google-chrome-on-centos/>

添加谷歌官方yum源，在CentOS的`/etc/yum.repos.d/`目录中新建一个`google-chrome.repo`文件，内容如下：

	[google-chrome]
	name=google-chrome
	baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
	enabled=1
	gpgcheck=1
	gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

执行如下命令进行安装：

	sudo yum install google-chrome-stable

验证不成功的话，使用如下命令：

	sudo yum install google-chrome-stable --nogpgcheck        # 安装时忽略GPG验证

如果遇到下面这种谷歌网站无法访问的情形，可以按照如下设置使用代理上网。

	failure: repodata/repomd.xml from google-chrome: [Errno 256] No more mirrors to try.
	http://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/repomd.xml: [Errno 14] curl#6 - "Could not resolve host: dl.google.com; Unknown error"

编辑`/etc/yum.conf`，配置`yum`代理，在文件末尾增加以下内容：

	proxy=http://你的代理IP:端口号
	# 如果需要连接认证，添加以下配置
	#proxy_username=代理服务器用户名 
	#proxy_password=代理服务器密码

编辑`/etc/profile`，为系统设置全局代理，增加如下环境变量：

	export http_proxy=http://124.156.182.201:80
	export https_proxy=http://124.156.182.201:80

保存配置，并执行如下命令使环境变量立即生效：

	source /etc/profile

使用上述方法，默认会安装最新版本的chrome，如果想重装其他的版本，可以下载相应版本chrome的rpm包来安装。

	[root@localhost src]# rpm -qa | grep chrome # 查看当前安装的chrome版本
	google-chrome-stable-75.0.3770.90-1pclos2019.x86_64
	[root@localhost src]# rpm -e google-chrome-stable-75.0.3770.90-1pclos2019.x86_64  # 卸载当前安装的chrome版本
	[root@localhost src]# rpm -ivh google-chrome-58.0.3029.110_x86_64.rpm  # 重新安装chrome

# 下载chromedriver
使用如下命令查看你安装的chrome版本：

	[root@localhost /]# google-chrome --version
	Google Chrome 75.0.3770.90

chromedriver与chrome版本对应关系见下表：

<table><tbody><tr><td> <p><strong>chromedriver版本</strong></p> </td><td> <p><strong>支持的chrome版本</strong></p> </td></tr><tr><td> <p>v2.43</p> </td><td> <p>v69-71</p> </td></tr><tr><td> <p>v2.42</p> </td><td> <p>v68-70</p> </td></tr><tr><td> <p>v2.41</p> </td><td> <p>v67-69</p> </td></tr><tr><td> <p>v2.40</p> </td><td> <p>v66-68</p> </td></tr><tr><td> <p>v2.39</p> </td><td> <p>v66-68</p> </td></tr><tr><td> <p>v2.38</p> </td><td> <p>v65-67</p> </td></tr><tr><td> <p>v2.37</p> </td><td> <p>v64-66</p> </td></tr><tr><td> <p>v2.36</p> </td><td> <p>v63-65</p> </td></tr><tr><td> <p>v2.35</p> </td><td> <p>v62-64</p> </td></tr><tr><td> <p>v2.34</p> </td><td> <p>v61-63</p> </td></tr><tr><td> <p>v2.33</p> </td><td> <p>v60-62</p> </td></tr><tr><td> <p>v2.32</p> </td><td> <p>v59-61</p> </td></tr><tr><td> <p>v2.31</p> </td><td> <p>v58-60</p> </td></tr><tr><td> <p>v2.30</p> </td><td> <p>v58-60</p> </td></tr></tbody></table>

各版本chromedriver下载地址：

<http://chromedriver.storage.googleapis.com/index.html>

<http://npm.taobao.org/mirrors/chromedriver/>

在下载地址中找到与你的chrome对应的chromedriver版本并下载、解压：

	wget http://chromedriver.storage.googleapis.com/75.0.3770.90/chromedriver_linux64.zip
	unzip chromedriver_linux64.zip

将下载好的chromedriver放到python脚本同级目录方便调用，并修改读写权限：

	chmod 755 chromedriver

# 代码测试
创建一个`test.py`文件，内容如下：

	# -*- coding: utf-8 -*-
	from selenium import webdriver
	 
	print("开始爬取")
	# 创建chrome参数对象
	options = webdriver.ChromeOptions()
	options.add_argument('--no-sandbox') # 解决DevToolsActivePort文件不存在的报错
	options.add_argument('window-size=1600x900') # 指定浏览器分辨率
	options.add_argument('--disable-gpu') # 谷歌文档提到需要加上这个属性来规避bug
	options.add_argument('--hide-scrollbars') # 隐藏滚动条, 应对一些特殊页面
	options.add_argument('blink-settings=imagesEnabled=false') # 不加载图片, 提升速度
	options.add_argument('--headless') # 浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
	 
	brower = webdriver.Chrome(options=options,executable_path='./chromedriver')
	 
	brower.get('http://www.baidu.com')
	print(brower.title)
	# print(brower.page_source)
	brower.quit()
	print("爬取完成")

命令行执行`python test.py`，启动脚本，输出如下内容则证明Selenium+chromedriver安装成功：

	[root@localhost python_projects]# python3 test.py
	开始爬取
	百度一下，你就知道
	爬取完成

# 参考文章

<https://blog.csdn.net/weixin_40476348/article/details/85992293>

<https://www.cnblogs.com/maikatsura/p/10565989.html>

<https://intoli.com/blog/installing-google-chrome-on-centos/>

<https://www.cnblogs.com/zouke1220/p/9326687.html>

<https://developers.google.com/web/updates/2017/04/headless-chrome>