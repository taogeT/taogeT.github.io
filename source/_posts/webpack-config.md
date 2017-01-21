---
title: webpack配置使用
tags:
  - search
  - skill
categories: 研究心得
date: 2017-01-21 22:35:23
---


### webpack打包单页前端项目
***

在项目 [直播爬虫](https://github.com/taogeT/livetv_mining) 的前端开发过程中，使用了webpack集成打包，实现了单页面前端的效果。特写此文总结webpack使用经验与配置。

<!-- more -->

#### webpack特点
***

webpack对于各种主流模块的安装依赖、不同规范语法(amd/cmd/commonjs)，以及不同的模块加载器和插件等都有很好的支持，开发人员可以通过配置轻松打包一个项目，减少学习成本。或者换句话说，对于我这样前端开发接触不多的开发者，webpack提供了一整套方便处理的编译打包测试方案。

一切资源都可以模块化的工作方式，也很符合后端开发者一贯思维方式。同时还提供了丰富的插件供选择，基本上可以满足前端开发需求。

上面提到的都是webpack优势，现在来说说它的不足。首先webpack项目事件不是很长，目前1.x稳定版2.x开发版，我稍微看了下内容担心会像angular一样会有跨度大，迁移学习成本高的问题。第二点webpack本身配置和使用文档内容很繁琐，且质量不高，更别提其他的第三方插件了，我使用下来感觉不是很顺畅，初期学习成本比较陡峭。

与其他几种主流打包比较，建议看下这篇文章 [Module loader comparison](http://hackhat.com/p/110/module-loader-webpack-vs-requirejs-vs-browserify/)。

反正我自己是在用了webpack后把gulp全删了，懒！

#### 配置介绍(持续更新)
***

以直播项目配置为例，介绍如何配置webpack。[官网](http://webpack.github.io/docs/configuration.html)

目前分为三个文件：base(基础)，dev(开发)，prod(生产)。

dev与prod的区别主要是在plugins上：

* dev中包含了热启动和webpack-dev-server启动的信息。

    webpack.optimize.OccurrenceOrderPlugin()
    webpack.HotModuleReplacementPlugin()
    webpack.NoErrorsPlugin()

* prod包含了混淆css、js文件，以及html文件压缩处理。

    webpack.optimize.UglifyJsPlugin({compress: {warnings: false}})


