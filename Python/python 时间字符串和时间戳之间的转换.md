> 原文链接：<https://blog.csdn.net/qq_37193537/article/details/78987949>

# 将字符串的时间转换为时间戳

	    a = "2013-10-10 23:40:00"
	    # 将其转换为时间数组
	    import time
	    timeArray = time.strptime(a, "%Y-%m-%d %H:%M:%S")
	    # 转换为时间戳
	    timeStamp = int(time.mktime(timeArray))
	    timeStamp == 1381419600

# 字符串格式更改

如：a = "2013-10-10 23:40:00",想改为 a = "2013/10/10 23:40:00"

方法: 先转换为时间数组,然后转换为其他格式    

	timeArray = time.strptime(a, "%Y-%m-%d %H:%M:%S")
	otherStyleTime = time.strftime("%Y/%m/%d %H:%M:%S", timeArray)

# 时间戳转换为指定格式日期:

方法一:利用localtime()转换为时间数组,然后格式化为需要的格式,如

	timeStamp = 1381419600
	timeArray = time.localtime(timeStamp)
	otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
	otherStyletime == "2013-10-10 23:40:00"

方法二:

	importdatetime
	timeStamp = 1381419600
	dateArray = datetime.datetime.utcfromtimestamp(timeStamp)
	otherStyleTime = dateArray.strftime("%Y-%m-%d %H:%M:%S")
	otherStyletime == "2013-10-10 23:40:00"

# 获取当前时间并转换为指定日期格式

方法一:
	
	import time
	
	# 获得当前时间时间戳
	now = int(time.time())  # ->这是时间戳
	# 转换为其他日期格式,如:"%Y-%m-%d %H:%M:%S"
	timeArray = time.localtime(timeStamp)
	otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)

方法二:

	import datetime
	
	# 获得当前时间
	now = datetime.datetime.now()  #->这是时间数组格式
	# 转换为指定的格式:
	otherStyleTime = now.strftime("%Y-%m-%d %H:%M:%S")

# 获得三天前的时间
        
	import time
 
    import datetime

    # 先获得时间数组格式的日期
    threeDayAgo = (datetime.datetime.now() - datetime.timedelta(days = 3))

    # 转换为时间戳:
    timeStamp = int(time.mktime(threeDayAgo.timetuple()))

    # 转换为其他字符串格式:
    otherStyleTime = threeDayAgo.strftime("%Y-%m-%d %H:%M:%S")

<font color=red>注</font>:`timedelta()`的参数有:days,hours,seconds,microseconds

# 给定时间戳,计算该时间的几天前时间

    timeStamp = 1381419600
 
    # 先转换为datetime
 
    import datetime
    import time
 
    dateArray = datetime.datetime.utcfromtimestamp(timeStamp)
    threeDayAgo = dateArray - datetime.timedelta(days = 3)

参考前一章,可以转换为其他的任意格式了

对于时间之间的格式：

	%a 星期的简写。如 星期三为Web
	%A 星期的全写。如 星期三为Wednesday
	%b 月份的简写。如4月份为Apr
	%B 月份的全写。如4月份为April
	%c:  日期时间的字符串表示。（如： 04/07/10 10:43:39）
	%d:  日在这个月中的天数（是这个月的第几天）
	%f:  微秒（范围[0,999999]）
	%H:  小时（24小时制，[0, 23]）
	%I:  小时（12小时制，[0, 11]）
	%j:  日在年中的天数 [001,366]（是当年的第几天）
	%m:  月份（[01,12]）
	%M:  分钟（[00,59]）
	%p:  AM或者PM
	%S:  秒（范围为[00,61]，为什么不是[00, 59]，参考python手册~_~）
	%U:  周在当年的周数当年的第几周），星期天作为周的第一天
	%w:  今天在这周的天数，范围为[0, 6]，6表示星期天
	%W:  周在当年的周数（是当年的第几周），星期一作为周的第一天
	%x:  日期字符串（如：04/07/10）
	%X:  时间字符串（如：10:43:39）
	%y:  2个数字表示的年份
	%Y:  4个数字表示的年份
	%z:  与utc时间的间隔 （如果是本地时间，返回空字符串）
	%Z:  时区名称（如果是本地时间，返回空字符串）
	%%:  %% => %
 