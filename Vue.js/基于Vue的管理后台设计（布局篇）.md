文章系列：

- 基于Vue的管理后台设计（布局篇）
- 基于Vue的管理后台设计（登录鉴权篇）
- 基于Vue的管理后台设计（打包部署篇）

# 前言 
我打算把接下来要写的几篇文章写成一个系列，用来记录一下如何基于Vue一步一步地搭建一个后台管理系统。 

文章前传：

- 《Vue组件中引入jQuery》
- 《Webpack项目中引入Bootstrap4.x》
- 《Webpack项目中使用ToolTipster》

本篇文章就是这个系列的第一篇，将对整个系统的页面布局进行设计。在菜单目录树的实现中我们还将使用到前一篇《前端无小事--Webpack项目中使用ToolTipster》中创建好的提示框组件，最终实现的页面效果如下。
<div align=center>

![Vue.js](./imgs/68.gif "Vue.js示意图")
<div align=left>

# Layout.vue
整个页面的布局分为三块区域：左侧菜单区、顶部菜单栏和内容区。
<div align=center>

![Vue.js](./imgs/69.png "Vue.js示意图")
<div align=left>

下面是它的完整代码：

	<template>
	  <div class="layout">
	    <div class="left-container" :style="{width: status.isCollapsed?'64px':'200px'}">
	      <div class="logo-wrapper">
	        <img style="width:50px;height:50px;" src="./DigitalX1.png" />
	      </div>
	      <div class="menu-wrapper">
	        <v-menu v-for="(menu,index) in this.menu_list" :key="index" :menu="menu" :status="status"></v-menu>
	      </div>
	    </div>
	 
	    <div class="topbar-container" :style="{left: this.status.isCollapsed?'64px':'200px'}">
	      <div class="btn btn-mini btn-success" @click="collapsed">
	        <i class="icon-exchange"></i>
	      </div>
	    </div>
	    <div class="content-container" :style="{left: this.status.isCollapsed?'64px':'200px'}">
	      <div class="content">
	        <router-view></router-view>
	      </div>
	    </div>
	  </div>
	</template>
	<script>
	import $ from "jquery";
	import MenuItem from "../menu/MenuItem";
	 
	export default {
	  components: {
	    "v-menu": MenuItem
	  },
	  data() {
	    return {
	      menu_list: [
	        {
	          path: "/home",
	          title: "首页",
	          icon: "icon-home icon-large"
	        },
	        {
	          path: "/user",
	          title: "用户管理",
	          icon: "icon-user icon-large",
	          children: [
	            { path: "/user/roles", title: "用户角色" },
	            { path: "/user/auths", title: "用户权限" }
	          ]
	        },
	        {
	          path: "/sys",
	          title: "系统管理",
	          icon: "icon-heart icon-large",
	          children: [
	            { path: "/sys/jobs", title: "定时任务" },
	            { path: "/sys/menus", title: "菜单管理" }
	          ]
	        }
	      ],
	      status: {
	        isCollapsed: false,
	        currentMenu: "首页",
	        parentMenu: "首页"
	      }
	    };
	  },
	  methods: {
	    collapsed: function() {
	      if (this.status.isCollapsed) {
	        this.status.isCollapsed = false;
	        $(".l2").removeClass("hidden");
	      } else {
	        this.status.isCollapsed = true;
	        $(".l2").addClass("hidden");
	      }
	    }
	  }
	};
	</script>
	<style scoped>
	.left-container {
	  position: fixed;
	  top: 0;
	  bottom: 0;
	  left: 0;
	  z-index: 99;
	  background-color: #f6f6f6;
	  transition: all 0.3s ease-in-out;
	  box-shadow: 0 2px 4px 0 rgba(96, 125, 139, 0.9),
	    0 0 6px 0 rgba(96, 125, 139, 0.4);
	}
	.logo-wrapper {
	  display: block;
	  margin: 20px 5px;
	  text-align: center;
	}
	.topbar-container {
	  position: fixed;
	  right: 0;
	  top: 0;
	  height: 48px;
	  line-height: 48px;
	  padding: 0 10px;
	  background-color: #f6f6f6;
	  box-shadow: 0 2px 4px 0 rgba(96, 125, 139, 0.9),
	    0 0 6px 0 rgba(96, 125, 139, 0.4);
	  transition: all 0.3s ease-in-out;
	  z-index: 99;
	}
	.content-container {
	  position: fixed;
	  right: 0;
	  top: 48px;
	  bottom: 0;
	  padding: 16px;
	  overflow: auto;
	  transition: all 0.3s ease-in-out;
	}
	</style>

# MenuItem.vue
MenuItem是一个自定义的菜单组件，每一个一级菜单都会被渲染成一个MenuItem实例，并包含了该一级菜单下面的二级菜单。

	<template>
	  <div class="menu">
	    <div class="menu-box l1">
	      <vue-tooltipster
	        :tooltipsterOptions="{side:'right'}"
	        v-if="hasChildren && status.isCollapsed"
	      >
	        <div
	          :class="menu.title==status.parentMenu || menu.title==status.currentMenu? 'menu-item current-menu':'menu-item'"
	          @click="menuClick($event)"
	        >
	          <span class="icon-span">
	            <i v-bind:class="menu.icon"></i>
	          </span>
	        </div>
	        <div slot="content">
	          <li
	            class="tip-menu"
	            v-for="(submenu,index) in menu.children"
	            :key="index"
	            :title="submenu.title"
	            @click="tipMenuClick($event)"
	          >
	            <span>{{submenu.title}}</span>
	          </li>
	        </div>
	      </vue-tooltipster>
	      <div
	        v-else
	        :class="menu.title==status.currentMenu? 'menu-item current-menu':'menu-item'"
	        @click="menuClick($event)"
	      >
	        <span class="icon-span">
	          <i v-bind:class="menu.icon"></i>
	        </span>
	        <span v-if="!status.isCollapsed">{{menu.title}}</span>
	        <span class="arrow-span" v-if="hasChildren && !status.isCollapsed">
	          <i v-if="showSubMenu" class="menu-arrow icon-angle-up icon-large"></i>
	          <i v-else class="menu-arrow icon-angle-down icon-large"></i>
	        </span>
	      </div>
	      <ul class="menu-box l2" v-if="showSubMenu && hasChildren && !status.isCollapsed">
	        <li
	          v-for="(submenu,index) in menu.children"
	          :key="index"
	          :class="submenu.title==status.currentMenu? 'menu-item current-menu':'menu-item'"
	          :title="submenu.title"
	          @click="subMenuClick($event)"
	        >
	          <span class="icon-span"></span>
	          <span v-if="!status.isCollapsed">{{submenu.title}}</span>
	        </li>
	      </ul>
	    </div>
	  </div>
	</template>
	<script>
	import $ from "jquery";
	import VueTooltipster from "../tooltip/V-ToolTip";
	 
	export default {
	  data() {
	    return {
	      showSubMenu: this.menu.title == this.status.parentMenu
	    };
	  },
	  components: {
	    VueTooltipster
	  },
	  props: ["menu", "status"],
	  computed: {
	    hasChildren: function() {
	      return this.menu.children && this.menu.children.length > 0;
	    }
	  },
	  methods: {
	    menuClick: function(event) {
	      var $this = $(event.currentTarget);
	      var $arrow = $this.find(".menu-arrow");
	      if (this.hasChildren) {
	        if ($arrow.hasClass("icon-angle-down")) {
	          $arrow.removeClass("icon-angle-down").addClass("icon-angle-up");
	          this.showSubMenu = true;
	        } else {
	          $arrow.removeClass("icon-angle-up").addClass("icon-angle-down");
	          this.showSubMenu = false;
	        }
	      } else {
	        this.status.parentMenu = this.menu.title;
	        this.status.currentMenu = this.menu.title;
	        $(".menu-item").removeClass("current-menu");
	        $this.addClass("current-menu");
	        console.log("进入菜单:"+this.status.currentMenu);
	      }
	    },
	    subMenuClick: function(event) {
	      var $this = $(event.currentTarget);
	      $(".menu-item").removeClass("current-menu");
	      $this.addClass("current-menu");
	      this.status.parentMenu = this.menu.title;
	      this.status.currentMenu = $this.attr("title");
	      console.log("进入菜单:"+this.status.currentMenu);
	    },
	    tipMenuClick: function(event) {
	      var $this = $(event.currentTarget);
	      this.status.currentMenu = $this.attr("title");
	      this.status.parentMenu = this.menu.title;
	      console.log("进入菜单:"+this.status.currentMenu);
	    }
	  }
	};
	</script>
	<style scoped>
	.menu-box {
	  text-align: left;
	  list-style: none;
	  margin: 0;
	  padding: 0;
	}
	.menu-item {
	  cursor: pointer;
	  color: black;
	  line-height: 48px;
	  white-space: nowrap;
	  list-style: none;
	}
	.menu-item:hover {
	  color: #7bb6e4;
	  background-color: rgba(40, 167, 69, 0.5);
	}
	.current-menu {
	  color: white !important;
	  background-color: #28a745 !important;
	}
	.tip-menu {
	  cursor: pointer;
	  color: black;
	  line-height: 48px;
	  white-space: nowrap;
	  list-style: none;
	}
	.tip-menu:hover {
	  color: #28a745;
	}
	.icon-span {
	  display: inline-block;
	  width: 64px;
	  padding-left: 22px;
	}
	.arrow-span {
	  display: inline-block;
	  position: absolute;
	  right: 20px;
	}
	.hidden {
	  display: none !important;
	}
	</style>

`==============================我是分割线===============================`

如果你想和我一样自己写一个目录树组件，我的建议是 最好不要自己写 ，原因很简单 可能出现的意外太多了，你很难能考虑到所有情况。所以，你的目录树组件好用不好用还不一定。我上面自己写的那个组件在实际使用时就出现了问题：当用户直接在地址栏输入地址时，如何定位到菜单；当用户输入一个错误的地址时，菜单要如何展示；云云。。。

明智的做法是使用开源的组件，省得去重复造轮子，关键是稳定性也是经过大家考验的。

例如，我使用的是[Element](https://element.eleme.cn/#/zh-CN "Element")，对上面的布局进行调整后代码如下：

	<template>
	  <div class="layout">
	    <div class="left-container" :style="{width: status.isCollapsed?'64px':'200px'}">
	      <div class="logo-wrapper">
	        <img style="width:50px;height:50px;" src="./DigitalX1.png" />
	      </div>
	      <div class="menu-wrapper">
	        <el-menu
	          text-color="#000000"
	          active-text-color="#ffffff"
	          router
	          unique-opened
	          :collapse="status.isCollapsed"
	          :default-active="$route.path"
	        >
	          <template v-for="(menu, index) in menu_list">
	            <el-menu-item class="menu-item" v-if="!menu.children" :index="menu.path" :key="index">
	              <i :class="menu.icon" style="font-size:24px;"></i>
	              <span slot="title">{{menu.title}}</span>
	            </el-menu-item>
	            <el-submenu v-else :index="menu.path">
	              <template slot="title">
	                <i :class="menu.icon" style="font-size:24px;"></i>
	                <span slot="title">{{menu.title}}</span>
	              </template>
	              <el-menu-item
	                class="menu-item"
	                v-for="(subMenu, subIndex) in menu.children"
	                :index="subMenu.path"
	                :key="subIndex"
	              >
	                <span slot="title" style="margin-left:13px;">{{subMenu.title}}</span>
	              </el-menu-item>
	            </el-submenu>
	          </template>
	        </el-menu>
	      </div>
	    </div>
	 
	    <div class="topbar-container" :style="{left: this.status.isCollapsed?'64px':'200px'}">
	      <div class="el-button el-button--default el-button--small" @click="collapsed">
	        <i id="collapsedIcon" class="el-icon-s-fold"></i>
	      </div>
	    </div>
	    <div class="content-container" :style="{left: this.status.isCollapsed?'64px':'200px'}">
	      <div class="content" style="height: 100%;">
	        <router-view></router-view>
	      </div>
	    </div>
	  </div>
	</template>
	<script>
	export default {
	  data() {
	    return {
	      menu_list: [
	        {
	          path: "/home",
	          title: "首页",
	          icon: "el-icon-s-home"
	        },
	        {
	          path: "/user",
	          title: "用户管理",
	          icon: "el-icon-user-solid",
	          children: [
	            { path: "/user/roles", title: "用户角色" },
	            { path: "/user/auths", title: "用户权限" }
	          ]
	        },
	        {
	          path: "/sys",
	          title: "系统管理",
	          icon: "el-icon-s-tools",
	          children: [
	            { path: "/sys/jobs", title: "定时任务" },
	            { path: "/sys/menus", title: "菜单管理" }
	          ]
	        }
	      ],
	      status: {
	        isCollapsed: false,
	        parentMenu: "用户管理"
	      }
	    };
	  },
	  methods: {
	    collapsed: function() {
	      if (this.status.isCollapsed) {
	        this.status.isCollapsed = false;
	        $("#collapsedIcon")
	          .removeClass("el-icon-s-unfold")
	          .addClass("el-icon-s-fold");
	      } else {
	        this.status.isCollapsed = true;
	        $("#collapsedIcon")
	          .removeClass("el-icon-s-fold")
	          .addClass("el-icon-s-unfold");
	      }
	    }
	  }
	};
	</script>
	<style scoped>
	.left-container {
	  position: fixed;
	  top: 0;
	  bottom: 0;
	  left: 0;
	  z-index: 99;
	  background-color: #f6f6f6;
	  transition: all 0.3s ease-in-out;
	  box-shadow: 0 2px 4px 0 rgba(96, 125, 139, 0.9),
	    0 0 6px 0 rgba(96, 125, 139, 0.4);
	}
	.logo-wrapper {
	  display: block;
	  margin: 20px 5px;
	  text-align: center;
	}
	.topbar-container {
	  position: fixed;
	  right: 0;
	  top: 0;
	  height: 48px;
	  line-height: 48px;
	  padding: 0 10px;
	  background-color: #f6f6f6;
	  box-shadow: 0 2px 4px 0 rgba(96, 125, 139, 0.9),
	    0 0 6px 0 rgba(96, 125, 139, 0.4);
	  transition: all 0.3s ease-in-out;
	  z-index: 99;
	}
	.content-container {
	  position: fixed;
	  right: 0;
	  top: 48px;
	  bottom: 0;
	  padding: 16px;
	  overflow: auto;
	  transition: all 0.3s ease-in-out;
	}
	.menu-item.is-active {
	  background-color: #28a745 !important;
	}
	</style>

代码少了不少，是不是清爽了许多！！！