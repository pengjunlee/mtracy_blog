> 原文链接：<https://blog.csdn.net/webfront/article/details/80310763>

	// 首先 vue的点击事件 是用  @click = “clickfun()” 属性 在html中绑定的,
	// 在点击的函数中 添加$event 参数就可以
	// 比如:
	<button  @click = “clickfun($event)”>点击</button>
	 
	methods: {
	    clickfun(e) {
	      // e.target 是你当前点击的元素
	      // e.currentTarget 是你绑定事件的元素
	    }
	},