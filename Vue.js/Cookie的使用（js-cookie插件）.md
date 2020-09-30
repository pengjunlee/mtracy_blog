> 原文链接：<https://www.jianshu.com/p/6e1bacd35f59>

[js-cookie 官方文档](https://www.npmjs.com/package/js-cookie "js-cookie 官方文档")

里面就详细的介绍了`es5`怎么引用，以下是`ES6`以上的用户。

# 安装

	npm install js-cookie --save

# 引用

	import Cookies from 'js-cookie'

# 一般使用

## 存到Cookie去

	// Create a cookie, valid across the entire site:
	Cookies.set('name', 'value');
	 
	// Create a cookie that expires 7 days from now, valid across the entire site:
	Cookies.set('name', 'value', { expires: 7 });
	 
	// Create an expiring cookie, valid to the path of the current page:
	Cookies.set('name', 'value', { expires: 7, path: '' });

## 在Cookie中取出

	// Read cookie:
	Cookies.get('name'); // => 'value'
	Cookies.get('nothing'); // => undefined
	 
	// Read all visible cookies:
	Cookies.get(); // => { name: 'value' }

## 删除

	// Delete cookie:
	Cookies.remove('name');
	 
	// Delete a cookie valid to the path of the current page:
	Cookies.set('name', 'value', { path: '' });
	Cookies.remove('name'); // fail!
	Cookies.remove('name', { path: '' }); // removed!

# 特殊使用（在Cookie中存对象）
跟一般使用不同的是，从`Cookie`中取出的时候，要从字符串转换成`json`格式：

	const user = {
	  name: 'lia',
	  age: 18
	}
	Cookies.set('user', user)
	const liaUser = JSON.parse(Cookies.get('user'))
 