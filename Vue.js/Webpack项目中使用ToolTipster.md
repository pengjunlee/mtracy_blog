`Tooltipster`是一个简单易用且功能强大的`jQuery`提示框插件，用于对HTML元素的拖动、移入、点击等鼠标事件或其他键盘事件提供效果炫酷的提示框，有助于丰富系统的提示功能。本文主要记录一下如何在`webpack`项目中使用`ToolTipster`来构建自己的提示框组件。

官网地址：<http://down.admin5.com/demo/code_pop/28/72/>

官网示例：<https://vuejsexamples.com/vuejs-2-x-tooltip-component-based-tooltipster-js/>
<div align=center>

![Vue.js](./imgs/42.gif "Vue.js示意图")
<div align=left>

# 安装ToolTipster
在开始安装`ToolTipster`之前，我们需要先安装好`JQuery`和`Bootstrap4.x`。

《Webpack项目中引入Bootstrap4.x》

《Vue组件中引入jQuery》

本例中还用到了`Lodash`来合并`ToolTipster`的参数选项，使用NPM进行安装：

	npm install tooltipster --save // 安装ToolTipster
	npm install lodash --save // 安装Lodash

# 创建提示框组件
创建一个提示框组件，取名为`V-ToolTip.vue`，组件模板如下：

	<template>
	  <div class="v-tooltip">
	    <div ref="tooltipster">
	      <slot></slot>
	    </div>
	    <div class="hidden p-xs">
	      <div ref="content">
	        <slot name="content"></slot>
	      </div>
	    </div>
	  </div>
	</template>
	 
	<script>
	import "tooltipster/dist/css/tooltipster.bundle.min.css";
	import "tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-borderless.min.css";
	// import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-light.min.css'
	// import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-noir.min.css';
	// import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-punk.min.css'
	// import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-shadow.min.css'
	import $ from "jquery";
	import "tooltipster";
	import _ from "lodash";
	 
	export default {
	  name: "tooltip",
	  props: {
	    label: {
	      default: ""
	    },
	    tooltipsterOptions: {
	      type: Object,
	      default: function() {
	        return {};
	      }
	    }
	  },
	  data: function() {
	    return {
	      defaultOptions: {
	        animationDuration: 500,
	        animation: "fade",
	        contentAsHtml: true,
	        delay: 0,
	        minWidth: 100,
	        interactive: true,
	        distance: 10,
	        theme: "tooltipster-borderless", // tooltipster-shadow, tooltipster-light, tooltipster-borderless, tooltipster-punk, , tooltipster-noir
	        multiple: true,
	        respositionOnScroll: true,
	        trigger: "custom",
	        trackTooltip: false,
	        triggerOpen: {
	          mouseenter: true,
	          touchstart: true
	        },
	        triggerClose: {
	          mouseleave: true,
	          originClick: false,
	          touchleave: true
	        }
	      }
	    };
	  },
	  methods: {
	    beforeDestroy: function() {
	      this.tooltipsterInstance.close();
	    }
	  },
	  mounted: function() {
	    let self = this;
	    this.tooltipsterInstance = $(this.$refs.tooltipster.children[0])
	      .tooltipster(
	        _.merge(this.defaultOptions, this.tooltipsterOptions, {
	          content: this.label ? this.label : this.$refs.content
	        })
	      )
	      .tooltipster("instance");
	    this.tooltipsterInstance.on("close", function(event) {
	      console.log("close"); // 以后扩展备用
	    });
	 
	    this.tooltipsterInstance.on("before", function(event) {
	      console.log("before"); // 以后扩展备用
	    });
	 
	    this.tooltipsterInstance.on("after", function(event) {
	      console.log("after"); // 以后扩展备用
	    });
	  },
	  watch: {
	    label: function(newLabel) {
	      // if label change need to wath in order to update content to tooltipster!
	      if (newLabel) {
	        $(this.$refs.tooltipster.children[0]).tooltipster({
	          content: newLabel
	        });
	      } else {
	        $(this.$refs.tooltipster.children[0]).tooltipster({
	          content: _.get(this.$slots, "content[0].elm")
	        });
	      }
	    }
	  }
	};
	</script>
	<style scoped>
	.hidden {
	  display: none;
	}
	.p-xs {
	  padding: 5px;
	}
	</style>

# 使用提示框组件
在其他的VUE组件（`Hello.vue`）中导入提示框组件（`V-ToolTip.vue`）进行使用：

	<template>
	  <div class="demo">
	    <h1>Tooltipster组件的使用</h1>
	    <br />
	    <div>
	      <vue-tooltipster label="鼠标悬停在我这里。。。">
	        <span class="btn btn-small btn-info" style="margin:5px;">默认示例</span>
	      </vue-tooltipster>
	      <vue-tooltipster label="我的提示框在左边。。。" :tooltipsterOptions="{side: 'left'}">
	        <span class="btn btn-small btn-warning" style="margin:5px;">左侧示例</span>
	      </vue-tooltipster>
	      <vue-tooltipster label="我被点击了。。。" :tooltipsterOptions="{trigger: 'click',side:'right'}">
	        <span class="btn btn-small btn-primary" style="margin:5px;">点击示例</span>
	      </vue-tooltipster>
	      <vue-tooltipster :tooltipsterOptions="{side:'bottom'}">
	        <span class="btn btn-small btn-danger" style="margin:5px;">HTML示例</span>
	        <div slot="content" style="background-color: black">
	          <a href="https://blog.csdn.net/pengjunlee">
	            <i class="icon-home icon-large"></i>访问我的CSDN
	          </a>
	        </div>
	      </vue-tooltipster>
	    </div>
	  </div>
	</template>
	 
	<script>
	import VueTooltipster from "./V-ToolTip.vue";
	 
	export default {
	  name: "Hello",
	  data() {},
	  components: {
	    VueTooltipster
	  }
	};
	</script>
	<style scoped>
	</style>

在浏览器端访问`Hello`组件的效果如下图所示：
<div align=center>

![Vue.js](./imgs/43.png "Vue.js示意图")
<div align=left>

# 参考文章

<http://iamceege.github.io/tooltipster/>

<http://www.a5xiazai.com/demo/code_pop/28/72/>