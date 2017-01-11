---
title: Vue.js前端项目总结
tags:
  - Vue
  - search
categories: 规范
date: 2017-01-11 17:58:18
---


### 历程
***

近期完成了个人站点 [直播爬虫](https://www.zhengwentao.com/) 的前后端分离的改造，使用 [vue](https://github.com/vuejs/vue)+[vue-router](https://github.com/vuejs/vue-router)+[vuex](https://github.com/vuejs/vuex) 的方式来构建项目。分离过程中对规划组建前端项目有一点自己的心得体会。

<!-- more -->

### 工具
***

采用npm+webpack编译打包项目。webpack配置文件可参考[例子](https://github.com/taogeT/livetv_mining/blob/master/frontend/webpack.config.js)。

### 项目结构
***

以下为个人总结的项目结构，详细可查看[github](https://github.com/taogeT/livetv_mining/tree/master/frontend)。

![structure](/images/project-structure.png)

文件夹src内容解释：

* components 公共组件，通用页面/功能存放。
* filters 公共过滤器，通用过滤方法存放。
* resource RESTFul资源，目前采用[vue-resource](https://github.com/pagekit/vue-resource)管理资源。
* router 路由创建，由vue-router管理。
* store 全局持久数据存储，由vuex管理。
* views 页面组件，访问页面存放处，资源的调用仅能在此目录下的组件中进行。
* App.vue 根vue模块。
* main.js webpack打包根js脚本。

这样划分后基本涵盖了前端项目开发中涉及到的内容。

### 开发规范/经验(持续更新)
***

一些开发过程中个人遵循的规范和经验，可以比较好提升工作效率。

1. 公共组件不包含资源调用，数据仅由外部组件传入。
1. 在两个以上页面组件中出现重复html代码，该html代码必须拆分为公共组件。
1. 页面组件不得使用ajax获取数据，只能使用resource获取。
1. 页面组件获取数据只能与store交互，再通过store传递到其他页面组件。
1. 访问权限控制，通过路由 meta: { auth: ... } 管理。
1. 用户登录的场景，当用户登录后，store保存用户信息，router在每次切换时要验证session是否有效。若无效，删除store中用户信息。
