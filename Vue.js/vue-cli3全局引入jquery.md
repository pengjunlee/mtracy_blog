> 原文链接：<https://www.jianshu.com/p/5a196c2df13e>

之前使用`jquery`都是每个组件引入

	import $ from 'jquery'

这种方式需要在每个要使用`jquery`的组件里面引入一下，比较麻烦，分享一下全局引入的方法。

# 下载jquery

	npm install jquery

# 添加配置

`vue.config.js`中`webpack`配置`configureWebpack`添加`jquery`插件。

[vuecli3中修改webpack配置](https://links.jianshu.com/go?to=https%3A%2F%2Fcli.vuejs.org%2Fzh%2Fguide%2Fwebpack.html "vuecli3中修改webpack配置")：

	const webpack = require("webpack");
	module.exports = {
	    configureWebpack: {
	        //支持jquery
	        plugins: [
	            new webpack.ProvidePlugin({
	                $:"jquery",
	                jQuery:"jquery",
	                "windows.jQuery":"jquery"
	            })
	        ]
	    },
	};

# eslint配置

`package.json`中`eslint`配置项`env`中添加`"jquery":true`。

	"eslintConfig": {
	    "root": true,
	    "env": {
	      "node": true,
	      "jquery": true //此处配置意思为全局引入jquery，详情可查看文档
	    },
	    "extends": [
	      "plugin:vue/essential",
	      "eslint:recommended"
	    ],
	    "rules": {
	      "no-console": "off"
	    },
	    "parserOptions": {
	      "parser": "babel-eslint"
	    }
	},

大功告成，你就可以愉快在项目各个组件中不用`import`也可以使用`jquery`了。。