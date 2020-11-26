# 发送请求
使用`Requests`发送网络请求非常简单，支持的HTTP请求类型：`GET`，`POST`，`PUT`，`DELETE`，`HEAD`以及 `OPTIONS`。

	# Python version 3.6.5
	 
	# 导入 request 模块
	import requests
	 
	# 发送 GET 请求
	r = requests.get('http://httpbin.org/get')
	 
	# 发送 POST 请求
	# r = requests.post('http://httpbin.org/post', data = {'key':'value'})
	 
	# 发送 PUT 请求
	# r = requests.put('http://httpbin.org/put', data = {'key':'value'})
	 
	# 发送 DELETE 请求
	# r = requests.delete('http://httpbin.org/delete')
	 
	# 发送 HEAD 请求
	# r = requests.head('http://httpbin.org/get')
	 
	# 发送 OPTIONS 请求
	# r = requests.options('http://httpbin.org/get')

# 传递URL参数
Requests可以通过使用`params`选项来向URL中传递参数，`params`接受一个字符串字典类型的对象作为参数，举例来说，如果你想传递`key1=value1`和`key2=value2`到<http://httpbin.org/get>，那么你可以使用如下代码：

	params= {'key1': 'value1', 'key2': 'value2'}
	r = requests.get('http://httpbin.org/get',params=params)
	print(r.url)

打印结果：

	http://httpbin.org/get?key1=value1&key2=value2

你还可以将一个列表作为值传入：

	params= {'key1': 'value1', 'key2': ['value2', 'value3']}
	r = requests.get('http://httpbin.org/get', params=params)
	print(r.url)

打印结果：

	http://httpbin.org/get?key1=value1&key2=value2&key2=value3

<font color=red>注意</font>：字典里值为`None`的键都不会被添加到URL的查询字符串里。

# 响应内容
## 文本响应内容
我们能以文本形式来读取服务器响应的内容：

	r = requests.get('https://api.github.com/some/endpoint')
	 
	# r.text 获取字符串格式的响应内容
	print(r.text)

请求发出后，Requests会基于HTTP头部对响应的编码作出有根据的推测。当你访问`r.text`时，Requests会使用其推测的文本编码。你可以找出Requests使用了什么编码，并且能够使用`r.encoding`属性来改变它：

	# r.encoding 查看当前编码字符集
	print(r.encoding)
	 
	# 设置当前编码字符集
	r.encoding='utf-8'

<font color=red>注意</font>：在你需要的情况下，Requests也可以使用定制的编码。如果你创建了自己的编码，并使用`codecs`模块进行注册，你就可以轻松地使用这个解码器名称作为`r.encoding`的值， 然后由`Requests`来为你处理编码。
<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td>解码方式</td><td>返回类型</td><td>解码原理</td><td>修改编码</td></tr><tr><td>response.text</td><td>str</td><td>根据HTTP头部对响应的编码做出有根据的推测</td><td>response.encoding='gbk'</td></tr><tr><td>response.content.decode()</td><td>bytes</td><td>用户指定</td><td>response.content.decode('utf-8')</td></tr></tbody></table>
<font color=red>注意</font>：推荐使用 response.content.decode() 方式获取响应内容。

## 二进制响应内容
对于非文本请求，我们也能以字节的方式访问请求响应体，例如，以请求返回的二进制数据创建一张图片，你可以使用如下代码：

	from PIL import Image
	from io import BytesIO
	from matplotlib import pyplot
	 
	import requests
	 
	r = requests.get('https://www.baidu.com/img/baidu_jgylogo3.gif')
	 
	# r.content 获取二进制格式的响应内容
	image = Image.open(BytesIO(r.content))
	 
	#显示图片
	pyplot.imshow(image)
	pyplot.show()

Requests会自动为你解码`gzip`和`deflate`传输编码的响应数据。

你也可以使用如下代码把请求到的二进制图片保存到本地，不过请注意图片文件的扩展名格式要与请求地址中一致。

	# coding=utf-8
	import requests
	 
	# 发送请求
	response = requests.get('https://www.baidu.com/img/bd_logo1.png')
	 
	# 保存图片
	with open('a.png','wb') as f:
	    f.write(response.content)

## JSON 响应内容
Requests中有一个内置的JSON解码器，用于处理JSON格式的响应数据，

	r = requests.get('https://api.github.com/some/endpoint')
	 
	# r.json 获取JSON格式的响应内容
	print(r.json())

如果JSON解码失败，`r.json()`就会抛出一个异常。例如，响应内容是`401(Unauthorized)`，尝试访问`r.json()`将会抛出`ValueError: No JSON object could be decoded`异常。

需要注意的是，成功调用`r.json()`并不意味着响应的成功。有的服务器会在失败的响应中包含一个JSON对象（比如`HTTP 500`的错误细节）。这种JSON会被解码返回。要检查请求是否成功，请使用`r.raise_for_status()`或者检查 `r.status_code`是否和你的期望相同。

## 原始响应内容
我们还能够使用Request获取来自服务器的原始套接字响应：

	r = requests.get('https://www.baidu.com/img/baidu_jgylogo3.gif', stream=True)
	 
	# r.raw 获取原始响应内容
	print(r.raw)
	print(r.raw.read(10))
	 
	# 推荐使用以下面的模式将文本流保存到文件
	with open("D:\\res\\pachong\\response.txt", 'wb') as f:
	    for chunk in r.iter_content(1024):
	        f.write(chunk)

## 定制请求头
我们可以通过Requests的headers选项来为请求添加HTTP头部。

	cookie = 'SCF=Alooqnwebz90vN0JG1AwEZ...//......//...'
	headers = {
	'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; rv:24.0) Gecko/20100101 Firefox/24.0',
	'cookie': cookie
	}
	r = requests.get('https://api.github.com/some/endpoint', headers=headers)

## 添加请求内容
如果你想要在请求时发送一些编码为表单形式的数据，可以通过以下方式：

	import requests
	import json
	 
	# 方式一 字典数据
	data = {'key1': 'value1', 'key2': 'value2'}
	 
	# 方式二 元组数据
	data = (('key1', 'value1'), ('key1', 'value2'))
	 
	# 方式三 JSON数据
	data = {'some': 'data'}
	 
	r = requests.post("http://httpbin.org/post", data=json.dumps(data))
 
## 发送Multipart-Encoded文件
使用Requests上传多部分编码（Multipart-Encoded）文件非常简单：

	# 导入request模块
	import requests
	 
	url = 'http://httpbin.org/post'
	 
	# 显式地设置文件名，文件 MIME 类型和请求头
	files = {'file': ('login.js', open('login.js', 'rb'), 'application/x-javascript', {'Expires': '0'})}
	 
	# 将字符串作为文件发送
	# files = {'file': ('report.csv', 'some,data,to,send\nanother,row,to,send\n')}
	 
	r = requests.post(url, files=files)
	print(r.text)
 
## 响应状态码
Requests使用status_code检测响应状态码，`raise_for_status()`用来抛出异常：

	r = requests.get('http://httpbin.org/get')
	print(r.status_code)
	print(r.status_code == requests.codes.ok)
	 
	bad_r = requests.get('http://httpbin.org/status/404')
	print(bad_r.status_code)
	bad_r.raise_for_status()

## 响应头
我们可以查看以一个Python字典形式展示的服务器响应头：

	print(r.headers)
	{
	'content-encoding': 'gzip',
	'transfer-encoding': 'chunked',
	'connection': 'close',
	'server': 'nginx/1.0.4',
	'x-runtime': '148ms',
	'etag': '"e1ca502697e5c9317743dc078f67693f"',
	'content-type': 'application/json'
	}
	print(r.headers['Content-Type'])

## Cookie
如果某个响应中包含一些cookie，你可以快速访问它们：

	url = 'http://example.com/some/cookie/setting/url'
	r = requests.get(url)
	print(r.cookies['example_cookie_name'])

要想发送你的cookies到服务器，可以使用`cookies`参数：

	url = 'http://httpbin.org/cookies'
	cookies = dict(cookies_are='working')
	r = requests.get(url, cookies=cookies)
	print(r.text)

打印结果：

	{
	    "cookies": {
	        "cookies_are": "working"
	    }
	}

Cookie的返回对象为`RequestsCookieJar`，它的行为和字典类似，但接口更为完整，适合跨域名跨路径使用。你还可以把`Cookie Jar`传到Requests中：

	jar = requests.cookies.RequestsCookieJar()
	jar.set('tasty_cookie', 'yum', domain='httpbin.org', path='/cookies')
	jar.set('gross_cookie', 'blech', domain='httpbin.org', path='/elsewhere')
	url = 'http://httpbin.org/cookies'
	r = requests.get(url, cookies=jar)
	print(r.text)

打印结果：

	{
	    "cookies": {
	    "tasty_cookie": "yum"
	    }
	}

## 重定向与请求历史
在Request中，可以使用响应对象的`history`方法来追踪重定向。

默认情况下，除了`HEAD`,`Requests`会自动处理所有重定向，`Response.history`是一个`Response`对象的列表，为了完成请求而创建了这些对象。这个对象列表按照从最老到最近的请求进行排序。

例如，Github将所有的HTTP请求重定向到HTTPS：

	r = requests.get('http://github.com')
	 
	print(r.url)
	print(r.status_code)
	print(r.history)

打印结果：

	https://github.com/
	200
	[<Response [301]>]

如果你使用的是`GET`、`OPTIONS`、`POST`、`PUT`、`PATCH`或者`DELETE`，那么你可以通过`allow_redirects`参数禁用重定向处理：

	r = requests.get('http://github.com', allow_redirects=False)
	print(r.url)
	print(r.status_code)
	print(r.history)

打印结果：

	http://github.com/
	301
	[]

如果你使用了HEAD，你也可以启用重定向：

	r = requests.head('http://github.com', allow_redirects=True)
	print(r.url)
	print(r.status_code)
	print(r.history)

打印结果：

	https://github.com/
	200
	[<Response [301]>]

## 超时
你可以告诉requests在经过以`timeout`参数设定的秒数时间之后停止等待响应。基本上所有的生产代码都应该使用这一参数。如果不使用，你的程序可能会永远失去响应：

	r = requests.get('http://github.com', timeout=100)

<font color=red>注意</font>：timeout仅对连接过程有效，与响应体的下载无关。timeout并不是整个下载响应的时间限制，而是如果服务器在timeout秒内没有应答，将会引发一个异常（更精确地说，是在timeout秒内没有从基础套接字上接收到任何字节的数据时）`If no timeout is specified explicitly, requests do not time out.`

## 错误与异常
遇到网络问题（如：DNS查询失败、拒绝连接等）时，Requests会抛出一个`ConnectionError`异常。

如果HTTP请求返回了不成功的状态码，`Response.raise_for_status()`会抛出一个`HTTPError`异常。

若请求超时，则抛出一个`Timeout`异常。

若请求超过了设定的最大重定向次数，则会抛出一个`TooManyRedirects`异常。

所有Requests显式抛出的异常都继承自`requests.exceptions.RequestException`。

参考文章

<http://docs.python-requests.org/zh_CN/latest/user/quickstart.html>