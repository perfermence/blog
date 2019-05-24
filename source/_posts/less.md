---
title: less
date: 2019-04-21 22:59:16
tags:
- css
categories:
   - 笔记
---

### LESS
>它是 CSS预编译语言，和它类似的还有sass/stylus...
>
> CSS是标记语言，不是编程语言，没有类、实例、函数、变量等东西；而less等预编语言就是让CSS具备面向对象编程的思想；但是浏览器不能直接识别和渲染less代码，，需要我们把less代码预先编译为正常的CSS后，再交给浏览器渲染解析


### less的编译
- 在开发环境下编译（产品还未开发完，正在开发中，这个是开发环境（在本地开发））
  

> 在开发环境下，我们一般通过导入less插件来随时编译LESS代码
> 由于每一次加载页面都需要导入LESS.JS，并且把less文件重编译为CSS（很消耗性能，页面打开速度肯定会变慢），所以真实项目中，只有开发环境下我们使用这种模式，生产环境下，我们肯定需要事先把写好的LESS编译为正常的CSS后，在上线，以后用户访问的都是编译好的CSS，而不是拿less现编译
```
//=>rel=stylesheet/less
<link rel="stylesheet/less" ...>

//=>导入JS文件即可
<script src='js/less-2.5.3.min.js'></script>
```

-  在生产环境下编译（产品开发完成了，需要部署到服务器上，服务器上的环境叫做生产环境）
> 项目上线，不能把less部署，这样用户每一次打开页面都需要重新的编译，非常耗性能，我们部署到服务器上的是编译后的CSS
> 基于NODE编译
```
1、在当前电脑的全局环境下安装less模块
	 npm install less -g
	验证是否安装成功  lessc -v
2、基于命令把我们的less编译成CSS
	在less文件目录下执行DOS命令：
	 lessc xxx/xxx.less xxx/xxx.min.css -x 把指定目录中的less编译成为CSS（并且实现了代码的压缩），把编译后的CSS存入到具体指定路径中的文件中；上线前在HTML中导入的是CSS文件
```
- 目前基于webpack和框架实现工程化开发的时候，我们都在webpack配置文件中，配置出less的编译（需要安装less？less-loader等模块），这样不管是开发环境下的预览，还是部署到生产环境下，都是基于webpack中的less模块编译的