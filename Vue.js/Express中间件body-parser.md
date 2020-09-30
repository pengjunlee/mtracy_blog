> 原文链接：<https://www.jianshu.com/p/cd3de110b4b6>

# BodyParser简介
在`http`请求种，`POST`、`PUT`、`PATCH`三种请求方法中包含着请求体，也就是所谓的`request-body`，在`Nodejs`原生的`http`模块中，请求体是要基于流的方式来接受和解析。

`body-parser`是一个HTTP请求体解析的中间件，使用这个模块可以解析`JSON、Raw`、`文本`、`URL-encoded`格式的请求体。

Node原生的http模块中，是将用户请求数据封装到了用于请求的对象req中，这个对象是一个`IncomingMessage`，该对象同时也是一个可读流对象。在原生Http服务器，或不依赖第三方解析模块时，可以用下面的方法请求并且解析请求体：

    const http = require('http');
 
    http.createServer(function(req, res){
        if(req.method.toLowerCase() === 'post'){
            let body = '';
            //此步骤为接收数据
            req.on('data', function(chunk){
                body += chunk;
            });
            //开始解析
            req.on('end', function(){
                if(req.headers['content-type'].indexOf('application/json')!==-1){
                    JSON.parse(body);
                }else if(req.headers['content-type'].indexOf('application/octet-stream')!==-1){
                    //Rwa格式请求体解析
                }else if(req.headers['content-type'].indexOf('text/plain')!==-1){
                    //text文本格式请求体解析
                }else if(req.headers['content-type'].indexOf('application/x-www-form-urlencoded')!==-1){
                    //url-encoded格式请求体解析
                }else{
                //其他格式解析
                }
            })
        }else{
            res.end('其他方式提交')
        }
    }).listen(3000)

`Express`框架默认使用`body-parser`作为请求体解析中间件，在创建了Express项目之后，可以在`app.js`文件中找到：

	/* 引入依赖项 */
	var express = require('express');
	// ……
	var bodyParser = require('body-parser');
	 
	var routes = require('./routes/index');
	var users = require('./routes/users');
	 
	var app = express();
	 
	// ……
	 
	// 解析 application/json
	app.use(bodyParser.json()); 
	// 解析 application/x-www-form-urlencoded
	app.use(bodyParser.urlencoded());

这样就可以在项目的`application`级别，引入了`body-parser`模块处理请求体。在上述代码中，模块会处理`application/x-www-form-urlencoded`、`application/json`两种格式的请求体。经过这个中间件后，就可以在所有路由处理器的`req.body`中访问请求参数。

在实际项目中，不同路径可能要求用户使用不同的内容类型，`body-parser`还支持为单个`express`路由添加请求体解析，比如：

	var express = require('express');
	var bodyParser = require('body-parser');
	 
	var app = new express();
	 
	//创建application/json解析
	var jsonParser = bodyParser.json();
	 
	//创建application/x-www-form-urlencoded
	var urlencodedParser = bodyParser.urlencoded({extended: false});
	 
	//POST /login 中获取URL编码的请求体
	app.post('/login', urlencodedParser, function(req, res){
	    if(!req.body) return res.sendStatus(400);
	    res.send('welcome, ' + req.body.username);
	})
	 
	//POST /api/users 获取JSON编码的请求体
	app.post('/api/users', jsonParser, function(req,res){
	    if(!req.body) return res.sendStatus(400);
	    //create user in req.body
	})

# 指定请求类型
`body-parser`还支持为某一种或者某一类内容类型的请求体指定解析方式，指定时可以通过在解析方法中添加`type`参数修改指定`Content-Type`的解析方式。

比如，对`text/plain`内容类型使用JSON解析：

	app.use(bodyParser.json({type: 'text/plain'}))

这一选项更多是用在非标准请求头中的解析：

	// 解析自定义的 JSON
	app.use(bodyParser.json({ type: 'application/*+json' }))
	 
	// 解析自定义的 Buffer
	app.use(bodyParser.raw({ type: 'application/vnd.custom-type' }))
	 
	// 将 HTML 请求体做为字符串处理
	app.use(bodyParser.text({ type: 'text/html' }))

# body-parser模块的API
当请求体解析之后，解析值会被放到`req.body`属性中，当内容为空时候，为一个空对象`{}`。

- `bodyParser.json()`--解析JSON格式
- `bodyParser.raw()`--解析二进制格式
- `bodyParser.text()`--解析文本格式
- `bodyParser.urlencoded()`--解析文本格式
