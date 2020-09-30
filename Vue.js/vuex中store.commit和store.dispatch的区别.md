> 原文链接：<https://blog.csdn.net/AiHuanhuan110/article/details/89160241>

# 代码示例

	this.$store.commit('loginStatus', 1);
	this.$store.dispatch('isLogin', true);

# 规范的使用方式

	// 以载荷形式
	store.commit('increment'，{
	  amount: 10   //这是额外的参数
	})
	 
	// 或者使用对象风格的提交方式
	store.commit({
	  type: 'increment',
	  amount: 10   //这是额外的参数
	})

# 主要区别

- dispatch：含有异步操作，数据提交至 actions ，可用于向后台提交数据
- commit：同步操作，数据提交至 mutations ，可用于登录成功后读取用户信息写到缓存里

**写法示例**：

	this.$store.dispatch('isLogin', true);
	this.$store.commit('loginStatus', 1);

两者都可以以载荷形式或者对象风格的方式进行提交。

# 参考文章
[vuex的安装和简单使用](https://blog.csdn.net/AiHuanhuan110/article/details/89185816 "vuex的安装和简单使用")

[vuex使用总结和详解](https://blog.csdn.net/AiHuanhuan110/article/details/89184259#t7 "vuex使用总结和详解")