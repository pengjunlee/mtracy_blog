# Axios简介
`Axios`是一个基于`promise`的`HTTP`库，可以用在浏览器和`node.js`中，用来发送`HTTP`请求，其作用类似于`jQuery`的`ajax()`方法。

`Axios`支持如下特性：

- 从浏览器中创建 XMLHttpRequests
- 从`node.js`创建`http`请求
- 支持`Promise API`
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换`JSON`数据
- 客户端支持防御`XSRF`

# 安装
## 直接引用

	<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

## Npm安装

	npm install axios --save

# API
## axios(config)
该方法允许通过向`axios`传递一个JSON配置来创建请求。下面列举出了创建请求时可以用的配置选项。其中只有`url`是必需的。如果没有指定method，请求将默认使用`get`方法。

	{
	   // `url` 是用于请求的服务器 URL
	  url: '/user',
	 
	  // `method` 是创建请求时使用的方法
	  method: 'get', // default
	 
	  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
	  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
	  baseURL: 'https://some-domain.com/api/',
	 
	  // `transformRequest` 允许在向服务器发送前，修改请求数据
	  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
	  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
	  transformRequest: [function (data, headers) {
	    // 对 data 进行任意转换处理
	    return data;
	  }],
	 
	  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
	  transformResponse: [function (data) {
	    // 对 data 进行任意转换处理
	    return data;
	  }],
	 
	  // `headers` 是即将被发送的自定义请求头
	  headers: {'X-Requested-With': 'XMLHttpRequest'},
	 
	  // `params` 是即将与请求一起发送的 URL 参数
	  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
	  params: {
	    ID: 12345
	  },
	 
	   // `paramsSerializer` 是一个负责 `params` 序列化的函数
	  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
	  paramsSerializer: function(params) {
	    return Qs.stringify(params, {arrayFormat: 'brackets'})
	  },
	 
	  // `data` 是作为请求主体被发送的数据
	  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
	  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
	  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
	  // - 浏览器专属：FormData, File, Blob
	  // - Node 专属： Stream
	  data: {
	    firstName: 'Fred'
	  },
	 
	  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
	  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
	  timeout: 1000,
	 
	   // `withCredentials` 表示跨域请求时是否需要使用凭证
	  withCredentials: false, // default
	 
	  // `adapter` 允许自定义处理请求，以使测试更轻松
	  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
	  adapter: function (config) {
	    /* ... */
	  },
	 
	 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
	  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
	  auth: {
	    username: 'janedoe',
	    password: 's00pers3cret'
	  },
	 
	   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
	  responseType: 'json', // default
	 
	  // `responseEncoding` indicates encoding to use for decoding responses
	  // Note: Ignored for `responseType` of 'stream' or client-side requests
	  responseEncoding: 'utf8', // default
	 
	   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
	  xsrfCookieName: 'XSRF-TOKEN', // default
	 
	  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
	  xsrfHeaderName: 'X-XSRF-TOKEN', // default
	 
	   // `onUploadProgress` 允许为上传处理进度事件
	  onUploadProgress: function (progressEvent) {
	    // Do whatever you want with the native progress event
	  },
	 
	  // `onDownloadProgress` 允许为下载处理进度事件
	  onDownloadProgress: function (progressEvent) {
	    // 对原生进度事件的处理
	  },
	 
	   // `maxContentLength` 定义允许的响应内容的最大尺寸
	  maxContentLength: 2000,
	 
	  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
	  validateStatus: function (status) {
	    return status >= 200 && status < 300; // default
	  },
	 
	  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
	  // 如果设置为0，将不会 follow 任何重定向
	  maxRedirects: 5, // default
	 
	  // `socketPath` defines a UNIX Socket to be used in node.js.
	  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
	  // Only either `socketPath` or `proxy` can be specified.
	  // If both are specified, `socketPath` is used.
	  socketPath: null, // default
	 
	  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
	  // `keepAlive` 默认没有启用
	  httpAgent: new http.Agent({ keepAlive: true }),
	  httpsAgent: new https.Agent({ keepAlive: true }),
	 
	  // 'proxy' 定义代理服务器的主机名称和端口
	  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
	  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
	  proxy: {
	    host: '127.0.0.1',
	    port: 9000,
	    auth: {
	      username: 'mikeymike',
	      password: 'rapunz3l'
	    }
	  },
	 
	  // `cancelToken` 指定用于取消请求的 cancel token
	  // （查看后面的 Cancellation 这节了解更多）
	  cancelToken: new CancelToken(function (cancel) {
	  })
	}

请求返回的响应内容结构如下：

	{
	  // `data` 由服务器提供的响应
	  data: {},
	 
	  // `status` 来自服务器响应的 HTTP 状态码
	  status: 200,
	 
	  // `statusText` 来自服务器响应的 HTTP 状态信息
	  statusText: 'OK',
	 
	  // `headers` 服务器响应的头
	  headers: {},
	 
	   // `config` 是为请求提供的配置信息
	  config: {},
	 // 'request'
	  // `request` is the request that generated this response
	  // It is the last ClientRequest instance in node.js (in redirects)
	  // and an XMLHttpRequest instance the browser
	  request: {}
	}

请求示例：

	// 发送 GET 请求（默认的方法）
	axios('/user/89757');
	 
	// 发送 GET 请求，获取json数据
	axios("http://localhost:8080/package.json").then(res => { console.log(res.data) }).catch(error => { console.log("请求错误：" + error) });
	 
	// 发送 GET 请求，获取图片
	axios({ url: "http://localhost:8080/logo.png", method: "get", responseType: 'stream' }).then(res => { res.data.pipe(fs.createWriteStream('vue.png')) });
	 
	// 发送 POST 请求
	axios({ url: "http://localhost:8080/user/89757/update", method: "post", data: { name: "graython", salary: 5000 } }).then(res => { console.log(res.data) });

## 方法别名
为了方便调用，`Axios`为所有支持的请求方法提供了别名，分别对应八种请求类型。在使用别名方法时，`url`、`method`、`data`这些属性都不必在配置中指定。

	axios.request(config)
	axios.get(url[, config])
	axios.delete(url[, config])
	axios.head(url[, config])
	axios.options(url[, config])
	axios.post(url[, data[, config]])
	axios.put(url[, data[, config]])
	axios.patch(url[, data[, config]])

请求示例：

	axios.post("http://localhost:8080/user/89757/update", { name: "graython", salary: 5000 },{ headers: { token: "pengjunlee"} }).then(res => { console.log(res.data) });

## 并发
`Axios`还额外提供了两个用来处理并发请求的助手函数：

	axios.all(iterable)
	axios.spread(callback)

请求示例： 

	</header>
	    <script type="text/javascript">
	         // 发送 GET 请求，获取用户基本信息
	         function getUserInfos() {
	             return axios.get("http://localhost:8080/user/89757/infos");
	         };
	         // 发送 GET 请求，获取用户权限信息
	         function getUserPerms() {
	             return axios.get("http://localhost:8080/user/89757/perms");
	         };
	    </script>
	</header>

用户登录前同时获取用户基本信息和用户权限信息进行处理。

	beforeUserLogin: function () {
	    axios.all([ getUserInfos(), getUserPerms() ]).then(axios.spread(function (infos, perms)    {
	        // 此处可以对获取到的用户基本信息和用户权限信息进行处理
	        console.log(infos.data);
	        console.log(perms.data);
	    }))
	}

## 创建实例
你也可以把常用的配置新建一个`axios`实例，然后调用自定义的`axios`实例去发送请求。

请求示例： 

    const instance = axios.create({
      baseURL: 'http://localhost:8080/',
      timeout: 1000,
      headers: {'X-Custom-Header': 'foobar'}
    });
 
    // 发送get请求，获取json数据
    instance("package.json").then(res => { console.log(res.data) }).catch(error => { console.log("请求错误：" + error) });

## 拦截器
利用拦截器你可以在请求或响应被`then`或`catch`处理前拦截它们。

	// 添加请求拦截器
	axios.interceptors.request.use(function (config) {
	    // 在发送请求之前做些什么
	    return config;
	  }, function (error) {
	    // 对请求错误做些什么
	    return Promise.reject(error);
	  });
	 
	// 添加响应拦截器
	axios.interceptors.response.use(function (response) {
	    // 对响应数据做点什么
	    return response;
	  }, function (error) {
	    // 对响应错误做点什么
	    return Promise.reject(error);
	  });

如果你想在稍后移除拦截器，可以这样：

	const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
	axios.interceptors.request.eject(myInterceptor);

## 错误处理

	axios.get('/user/89757')
	  .catch(function (error) {
	    if (error.response) {
	      // The request was made and the server responded with a status code
	      // that falls out of the range of 2xx
	      console.log(error.response.data);
	      console.log(error.response.status);
	      console.log(error.response.headers);
	    } else if (error.request) {
	      // The request was made but no response was received
	      // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
	      // http.ClientRequest in node.js
	      console.log(error.request);
	    } else {
	      // Something happened in setting up the request that triggered an Error
	      console.log('Error', error.message);
	    }
	    console.log(error.config);
	  });

# 参考文章

<http://www.axios-js.com/zh-cn/docs/>