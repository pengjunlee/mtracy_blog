> 原文链接：<https://blog.csdn.net/wangcuiling_123/article/details/78513245>

使用Vue开发项目，用到`Element-UI`，根据官网的写法，我们可以自定义主题来适应我们的项目要求，下面来介绍一下两种方法实现的具体步骤，（可以参考[官方文档](https://element.eleme.cn/#/zh-CN/component/custom-theme "自定义主题官方文档")），先说项目中没有使用`scss`编写，用主题工具的方法（使用的较多）。

# 方法一：使用命令行主题工具
使用`vue-cli`安装完项目并引入`element-ui`（具体可参考第二种方法中的介绍）。

## 安装工具
首先安装`主题生成工具`。

	npm i element-theme -g

然后安装`chalk`主题，可以从`npm`安装或者从`GitHub`拉取最新代码。

	# 从 npm
	npm i element-theme-chalk -D
	 
	# 从 GitHub
	npm i https://github.com/ElementUI/theme-chalk -D

## 初始化变量文件

	et -i #[可以自定义变量文件，默认为element-variables.scss]
	> ✔ Generator variables file
	> 
如果使用默认配置，执行后当前目录会有一个`element-variables.scss`文件。内部包含了主题所用到的所有变量，它们使用`SCSS`的格式定义。大致结构如下：

	$--color-primary: #409EFF !default;
	$--color-primary-light-1: mix($--color-white, $--color-primary, 10%) !default; /* 53a8ff */
	$--color-primary-light-2: mix($--color-white, $--color-primary, 20%) !default; /* 66b1ff */
	$--color-primary-light-3: mix($--color-white, $--color-primary, 30%) !default; /* 79bbff */
	$--color-primary-light-4: mix($--color-white, $--color-primary, 40%) !default; /* 8cc5ff */
	$--color-primary-light-5: mix($--color-white, $--color-primary, 50%) !default; /* a0cfff */
	$--color-primary-light-6: mix($--color-white, $--color-primary, 60%) !default; /* b3d8ff */
	$--color-primary-light-7: mix($--color-white, $--color-primary, 70%) !default; /* c6e2ff */
	$--color-primary-light-8: mix($--color-white, $--color-primary, 80%) !default; /* d9ecff */
	$--color-primary-light-9: mix($--color-white, $--color-primary, 90%) !default; /* ecf5ff */
	 
	$--color-success: #67c23a !default;
	$--color-warning: #eb9e05 !default;
	$--color-danger: #fa5555 !default;
	$--color-info: #878d99 !default;
	 
	...

## 修改变量
直接编辑`element-variables.scss`文件，例如修改主题色为自己所需要的颜色（如： 紫色（purple））

	$--color-primary: purple;

## 编译主题
修改完变量后，要编译主题（如果编译后，再次修改了变量，需要重新编译）。

	et
	 
	> ✔ build theme font
	> ✔ build element theme

#引入自定义主题
最后一步，将编译好的主题文件引入项目（编译的文件默认在根目录下的`theme`文件下，也可以通过`-o`参数指定打包目录），在入口文件`main.js`中引入。

	import '../theme/index.css'
	import ElementUI from 'element-ui'
	import Vue from 'vue'
	 
	Vue.use(ElementUI)

在项目中写些样式，看下主题色是否改变：（主题色变为紫色）

	<div>
	      <el-button>默认按钮</el-button>
	      <el-button type="primary">主要按钮</el-button>
	      <el-button type="success">成功按钮</el-button>
	      <el-button type="info">信息按钮</el-button>
	      <el-button type="warning">警告按钮</el-button>
	      <el-button type="danger">危险按钮</el-button>
	</div>

# 方法二： 直接修改element样式变量
在项目中直接修改`element`的样式变量，（前提是你的文档也是使用`scss`编写）。

## 创建vue-cli项目
### 安装vue

	npm i -g vue

### 安装vue-cli

	npm i -g vue-cli

### 创建webpack项目（ vue-project）

	vue init webpack vue-project

### 启动项目
依次输入以下命令行，运行`vue-project`。

	cd vue-project
	npm i
	npm run dev

## 安装scss插件
安装`elementUI`以及`sass-loader`,`node-sass`(项目中使用`scss`编写需要依赖的插件)。

### 安装element-ui

	npm i element-ui -S

### 安装sass-loader，node-sass

	npm i sass-loader node-sass -D

在这里说一下，不需要配置`webpack.base.conf.js`文件,`vue-loader`会根据不同类型文件来配置相应`loader`来打包我们的样式文件（感兴趣的可看下`vue-loader`的核心代码）。

## 改变element样式变量
在`src`下建立`element-variables.scss`文件（名字可以自定义），写入如下代码：

	/* 改变主题色变量 */
	$--color-primary: teal;
	 
	/* 改变 icon 字体路径变量，必需 */
	$--font-path: '../node_modules/element-ui/lib/theme-chalk/fonts';
	 
	@import "../node_modules/element-ui/packages/theme-chalk/src/index";

在入口文件`main.js`中引入上面的文件即可

	import Vue from 'vue'
	import Element from 'element-ui'
	import './element-variables.scss'
	 
	Vue.use(Element)

看下效果吧，在文件里引入些样式看看，如`button`

	<div>
	      <el-button>默认按钮</el-button>
	      <el-button type="primary">主要按钮</el-button>
	      <el-button type="success">成功按钮</el-button>
	      <el-button type="info">信息按钮</el-button>
	      <el-button type="warning">警告按钮</el-button>
	      <el-button type="danger">危险按钮</el-button>
	</div>

默认的颜色已经变为我们自定义的了，有其他的改变在`element-variable.scss`文件中改变变量即可。