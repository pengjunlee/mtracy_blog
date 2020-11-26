# 认识logging
logging模块是Python内置的标准模块，主要用于输出运行日志，可以设置输出日志的等级、日志输出位置、日志文件回滚等。

在python中，logging由logger，handler，filter，formater四个部分组成：

## logger

logger为我们提供了记录日志的方法；

	def debug(self, msg, *args, **kwargs)
	def info(self, msg, *args, **kwargs)
	def warning(self, msg, *args, **kwargs)
	def error(self, msg, *args, **kwargs)
	def critical(self, msg, *args, **kwargs)

五个方法分别对应了五个日志等级，严重度从低到高依次为：DEBUG ，INFO ，WARNING ，ERROR， CRITICAL。

## handler
handler用来设置日志在什么地方输出，如：打印到控制台，写入到文件，邮件发送等，一个logger可以设置多个handler。

常用的handler类有如下几个：

- logging.StreamHandler # 将日志输出到流
- logging.FileHandler # 将日志输出到文件
- logging.handlers.BaseRotatingHandler # 回滚日志的handler基类，不能直接实例化，需要使用 RotatingFileHandler 或者 TimedRotatingFileHandler
- logging.handlers.RotatingFileHandler # 将日志输出到一系列文件，当正在记录的文件达到指定的大小时会立即切换到一个新文件中进行记录。
- logging.handlers.TimedRotatingFileHandler # 固定时间间隔对日志进行回滚，超过设置的日志备份数量时，删除最旧的日志文件
- logging.handlers.SMTPHandler # 将日志通过邮件发出，每一个logging事件都会发送一个SMTP邮件

## filter
filter过滤器使用户能够更加细粒度的控制日志的输出内容，例如，下面这段代码为logger增加了一个Shorten过滤器，用来将长度大于设定值的那些打印日志的类名或方法名以`...`省略显示。

	import logging
	 
	class Shorten(logging.Filter):
	    max_len = 7
	 
	    def filter(self, record):
	        # 当 %(name) 长度超过max_len的部分用 ... 显示
	        if len(record.name) > self.max_len:
	            record.name = record.name[:self.max_len] + '...'
	        return True
	 
	logging.basicConfig(format='[%(asctime)-15s] [%(levelname)8s] [%(name)10s ] - %(message)s (%(filename)s:%(lineno)s)',
	                    datefmt='%Y-%m-%d %T')
	 
	logger = logging.getLogger(__name__)
	 
	logger.setLevel(logging.WARNING)
	logger.addFilter(Shorten())
	 
	if __name__ == '__main__':
	    logger.debug('This is a debug message')
	    logger.info('This is a info message')
	    logger.warning('This is a warning message')
	    logger.error('This is a error message')

执行程序，日志打印效果如下： 

	[2019-04-30 10:29:53] [ WARNING] [__main_... ] - This is a warning message (filter_logger.py:23)
	[2019-04-30 10:29:53] [   ERROR] [__main_... ] - This is a error message (filter_logger.py:24)

## formater
formater用来设置日志的输出样式，常用的格式化参数如下：

	%(levelno)s: 打印日志级别的数值
	%(levelname)s: 打印日志级别名称
	%(pathname)s: 打印当前执行程序的路径，其实就是sys.argv[0]
	%(filename)s: 打印当前执行程序名
	%(funcName)s: 打印日志的当前函数
	%(lineno)d: 打印日志的当前行号
	%(asctime)s: 打印日志的时间
	%(thread)d: 打印线程ID
	%(threadName)s: 打印线程名称
	%(process)d: 打印进程ID
	%(message)s: 打印日志信息

其中的`asctime`时间格式可使用如下参数进行设置：

	%a - 简写的星期几
	%A - 星期名称(全称)
	%b - 缩写月份名
	%B - 完整的月份名称
	%c - 首选日期和时间表示
	%C - 世纪值(年份除以100，范围从00到99)
	%d - 每月第几天(01至31)
	%D -和 %m/%d/%y 一样
	%e - 月的一天(1〜31)
	%g - 类似 %G, 但没有世纪
	%G - 4位数年份对应ISO星期数(参见%V)。
	%h - 类似于 %b
	%H - 小时，采用24小时制(00〜23)
	%I - 小时，采用12小时制(00〜12)
	%j - 一年中的哪一天(001至366)
	%m - 月份(01〜12)
	%M - 分钟
	%n - 换行符
	%p - 根据给定的时间值判定上午或下午
	%r - 上午和下午(a.m 和 p.m)时间
	%R - 24小时制时间
	%S - 秒
	%t - 制表符
	%T - 当前时间，等于 %H:%M:%S
	%u - 工作日为数字(1至7)，星期一= 1。注：在Sun Solaris上 Sunday=1
	%U - 本年的周数，先从第一个星期日作为第一周的第一天
	%V - 本年度ISO 8601的周数(01到53)，其中第1周是在本年度至少4天的第一周，星期一作为一周的第一天
	%W - 本年周数，先从第一个星期一作为第一周的第一天
	%w - 一个星期中第几天，这是一个十进制数 Sunday=0
	%x - 无时间的日期表示
	%X - 无日期的首选时间表示
	%y - 无世纪的年份表示(00到99)
	%Y - 年份表示(包括世纪)
	%Z 或 %z - 时区或名称或缩写
	%% - 一个文字%字符

# 配置logging
在python中配置logging有如下三种方式：

## basicConfig
通过代码对logging进行配置，logging.basicConfig(**kwargs)函数中可设置如下参数：

- filename：指定日志文件名称，会将日志输出到文件；
- filemode：指定日志文件的打开模式，'w'或'a'；
- format：指定输出日志的输出内容和样式，设置方式参见 formater ；
- datefmt：指定时间格式，同time.strftime()；
- level：设置日志级别，默认为logging.WARNING；
- stream： 将日志输出到流，若 stream和filename同时指定时，stream将被忽略；

下面是一个配置示例：

	import logging
	 
	logging.basicConfig(level=logging.INFO,
	                    filename='./logs.log',
	                    filemode='a',
	                    format='[%(asctime)-15s] [%(levelname)8s] [%(name)10s ] - %(message)s (%(filename)s:%(lineno)s)',
	                    datefmt='%Y-%m-%d %T'
	                    )
	 
	logger = logging.getLogger(__name__)
	 
	if __name__ == '__main__':
	    logger.debug('This is a debug message')
	    logger.info('This is a info message')
	    logger.warning('This is a warning message')
	    logger.error('This is a error message')

执行这段代码，会将日志打印到 logs.log 文件中，日志格式如下：  

	[2019-04-30 09:01:17] [    INFO] [  __main__ ] - This is a info message (logger.py:23)
	[2019-04-30 09:01:17] [ WARNING] [  __main__ ] - This is a warning message (logger.py:24)
	[2019-04-30 09:01:17] [   ERROR] [  __main__ ] - This is a error message (logger.py:25)

## fileConfig
通过外部文件对logging进行配置，在 fileConfig(filename,defaults=None,disable_existing_loggers=Ture )中指定要读取的配置文件。

接下来，将上面的basicConfig示例改为从文件加载配置。

**编写配置文件**

在项目中新建一个`logging_conf.INI`配置文件，内容如下：

	[loggers]
	keys=root,fileLogger
	 
	[handlers]
	keys=consoleHandler,fileHandler
	 
	[formatters]
	keys=simpleFormatter
	 
	[logger_root]
	level=NOTSET
	handlers=consoleHandler
	 
	[logger_fileLogger]
	level=INFO
	handlers=fileHandler
	qualname=file.Logger
	propagate=0
	 
	[handler_consoleHandler]
	class=StreamHandler
	level=DEBUG
	formatter=simpleFormatter
	args=(sys.stdout,)
	 
	[handler_fileHandler]
	class=FileHandler
	level=INFO
	formatter=simpleFormatter
	args=('logs.log', 'a')
	 
	[formatter_simpleFormatter]
	format=[%(asctime)-15s] [%(levelname)8s] [%(name)10s ] - %(message)s (%(filename)s:%(lineno)s)
	datefmt=%Y-%m-%d %T
	class=logging.Formatter

**通过fileConfig()函数读取配置**

	# -- coding: UTF-8 --
	 
	import logging
	from logging import config
	 
	 
	logging.config.fileConfig('logging_conf.INI',)
	 
	# 获取限定名称为 file.Logger 的 logger
	logger = logging.getLogger('file.Logger')
	 
	if __name__ == '__main__':
	    logger.debug('This is a debug message')
	    logger.info('This is a info message')
	    logger.warning('This is a warning message')
	    logger.error('This is a error message')

执行程序，`logs.log`中打印的日志内容及日志格式与之前相同。

## dictConfig
通过传入的字典对logging进行配置，使用dictConfig(dict,defaults=None, disable_existing_loggers=Ture )函数来完成配置。

使用dictConfig对上例进行改造：

	# -- coding: UTF-8 --
	 
	import logging
	from logging import config
	 
	conf = {'version': 1,
	        'disable_existing_loggers': True,
	        'incremental': False,
	        'loggers': {
	            'rootLogger': {
	                'handlers': ['consoleHandler'],
	                'level': 'NOTSET'},
	            'fileLogger': {
	                'handlers': ['fileHandler', ],
	                'level': 'INFO',
	                'propagate': False}
	        },
	        'handlers': {
	            'consoleHandler': {
	                'class': 'logging.StreamHandler',
	                'level': 'DEBUG',
	                'formatter': 'simpleFormatter'},
	 
	            'fileHandler': {
	                'class': 'logging.FileHandler',
	                'level': 'INFO',
	                'formatter': 'simpleFormatter',
	                'filename': './logs.log'}
	        },
	        'formatters': {
	            'simpleFormatter': {
	                'format': '[%(asctime)-15s] [%(levelname)8s] [%(name)10s ] - %(message)s (%(filename)s:%(lineno)s)',
	                'datefmt': '%Y-%m-%d %T',
	                'class': 'logging.Formatter'
	            }
	        }
	 
	        }
	 
	logging.config.dictConfig(conf)
	 
	logger = logging.getLogger('fileLogger')
	 
	if __name__ == '__main__':
	    logger.debug('This is a debug message')
	    logger.info('This is a info message')
	    logger.warning('This is a warning message')
	    logger.error('This is a error message')

执行程序，`logs.log`中打印的日志内容及日志格式与之前依旧相同。

# 参考文章 

<https://docs.python.org/3/library/logging.config.html>

<https://docs.python.org/3/library/logging.html>