> 原文地址：<https://www.jianshu.com/p/df48debb771d>

# 脚本如下

	export LANG=zh_CN.UTF-8
	spider1='spider1'
	kill -9 `ps -ef | grep $spider1 | grep -v grep | awk '{print $2}'`
	cd ~/work/virtual/  # 切换到虚拟环境的目录,如果没有使用虚拟环境，则不需要
	/usr/local/bin/pipenv shell  # 激活虚拟环境
	cd ~/work/spider  # 进入scrapy爬虫项目的目录
	nohup ~/.local/share/virtualenvs/spider-MGYoPRkA/bin/scrapy crawl $spider1 > out.file 2>&1 &

#解释

- 由于crontab里没有tty/pts（终端）这个事实，即不会执行`$HOME/`下的一些环境初始工作，所以需要设置`export LANG=zh_CN.UTF-8`，不然会报<font color=red>UnicodeEncodeError: 'ascii' codec can't...</font>的错误；
- kill的作用为每次执行前，先停掉正在运行的爬虫；
- 由于crontab在运行时，有他自己的工作目录，所以需要切换到相应目录才能执行相应命令；

本人每五分钟跑一次，crontab代码为

	*/5 * * * * bash ~/work/start.sh

# 参考

[crontab文档](crontab文档 "http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/crontab.html")

<https://blog.csdn.net/kai404/article/details/53287563>