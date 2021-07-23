---

title: React学习（四）—— 脚手架

date: 2021-01-08 11:16:23

categories: React

tag: 

- React 
- HTML

---

# React学习（四）—— 脚手架

## 前端工程复杂化

如果我们只是开发几个小的demo，那么永远不需要考虑一些复杂的问题：

* 目录结构如何组织划分
* 如何管理文件之间的相互依赖
* 如何管理第三方模块的依赖
* 项目发布前端如何压缩、打包项目

<!--more-->

现代的前端项目越来越复杂了：

* 不再是之前那样，再HTML中一如几个`css`文件，引入几个编写的`js`文件或者第三方的`js`文件这么简单；
* 比如现在`css`可能是使用`less`、`sass`等预处理器进行编写的，我们需要将它们转成普通的css才能被浏览器解析；
* 比如`JavaScript`代码不再只是编写几个文件中，而是通过模块化的方式，被组成在**成百上千**个文件中，我们需要通过模块化的技术来管理它们之间的相互依赖；
* 比如项目需要依赖很多的第三方库，如何更好的管理它们（比如管理它们的依赖、版本升级等）

为了解决上面的这些问题，需要学习一些工具，比如babel、webpack、gulp，配置它们的转换规则，打包依赖，热更新等一些内容；

现在，还没开始工程化编写React，就需要首先学习一系列其他工具；脚手架的出现，可以解决上面的一系列问题。记得最开始学React的时候，不知道脚手架工具，网上找其他人的博客，复制粘贴各种Webpack配置，到最后总算折腾出来了，但是对于一个初学者来说，开发体验很不友好。有了脚手架，我们可以绕过很多问题。

## 脚手架

传统的脚手架是建筑学的一种结构，在搭建楼房、建筑物时，临时搭建出来的一个框架。

在编程中，提到脚手架`Scaffold`，其实是一种工具，帮助我们快速生成项目的工程化结构；每个项目完成的效果不同，但它们的基本工程化结构是很相似的，所以没有必要每次都从零开始搭建，完全可以使用一些工具，帮助我们生成基本的工程化模板；不同的项目，在这个模板的基础之上进行项目开发或者进行一些配置的简单修改即可。这样也可以间接保证项目的基本结构一致性。

脚手架让项目从搭建到开发，再到部署，整个流程变得快速和便捷。

前端流行的三大框架都有自己的脚手架：

* Vue脚手架`vue-cli`
* Angular脚手架`angular-cli`
* React脚手架`create-react-app`

什么是`npm`，全称Node Package Manager，即node包管理器，作用是帮助我们管理一些依赖包。

另外，还有一个Node包管理工具`yarn`，`yarn`是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具。`yarn` 是为了弥补`npm`的一些缺陷而出现的，早期的`npm`存在很多的缺陷，比如安装依赖速度很慢、版本依赖混乱等等一系列的问题；虽然从`npm`5版本开始，进行了很多的升级和改进，但是依然很多人喜欢使用`yarn`；React脚手架默认也是使用yarn。

### 创建React项目

创建React项目的命令如下：

```shell
create-react-app 项目名称
```

注意：项目名称不能包含大写字母。阮老师有讲这一点[为什么文件名要小写？](http://www.ruanyifeng.com/blog/2017/02/filename-should-be-lowercase.html)

进入项目可看到如下初始化结构：

![image-20210108071901560](https://raw.githubusercontent.com/ccbeango/blogImages/master/React/React%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%BC%80%E5%8F%91%E5%AE%9E%E8%B7%B505.png)

```html
02_learning_scaffold		
├── package.json
├── public								 公共资源目录
│   ├── favicon.ico				 	应用程序顶部的icon图标
│   ├── index.html					项目入口文件
│   ├── logo192.png					在manifest.json引用的文件
│   ├── logo512.png					在manifest.json引用的文件
│   ├── manifest.json        和Web App配置相关
│   └── robots.txt					指定搜索引擎可以或者无法爬取哪些文件
├── README.md
├── src
│   ├── App.css							APP组件相关的样式
│   ├── App.js							APP组件的代码文件
│   ├── App.test.js					APP组件的测试代码文件
│   ├── index.css						全局样式文件
│   ├── index.js						整个应用程序的入口文件
│   ├── logo.svg						启动项目，项目中的图标
│   ├── reportWebVitals.js	 
│   └── setupTests.js				 测试初始化文件
└── yarn.lock
```

### 了解PWA

之前的React版中中，`src`目录下会生成一个`serviceWorker.js`的文件（React16.13.1），这个文件的作用就是PWA。（React17.0.1中看不到了）。

PWA全称Progressive Web App，即渐进式WEB应用；一个 PWA 应用首先是一个网页, 可以通过 Web 技术编写出一个网页应用；一个 PWA 应用首先是一个网页, 可以通过 Web 技术编写出一个网页应用，随后添加上`App Manifest`和`Service Worker`来实现 PWA 的安装和离线等功能；这种Web存在的形式，也称之为Web App；

PWA解决的问题：

* 可以将应用添加至主屏幕，点击主屏幕图标可以实现启动动画以及隐藏地址栏。如安卓手机中，通过Chrome浏览一个PWA应用网页，可以将其添加到主屏幕，主屏幕也将会出现这个应用图标，下次可直接通过此图标访问PWA应用，就像手机上安装了一个新的APP一样。
* 实现离线缓存功能，即使用户手机没有网络，也可以使用一些离线功能
* 实现了消息推送，如安卓手机任务栏中的消息通知，PWA也可实现。
* 以及一系列类似于Native App相关的功能

`manifest.json`文件作用主要和Web App应用有关，主要是对安装到设备上的PWA应用做一些配置。

### 脚手架中的Webpack

Webpack 是一个现代 JavaScript 应用程序的静态模块打包器`module bundler`；当Webpack处理应用程序时，它会递归地构建一个依赖关系图`dependency graph`，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个`bundle`。

具体的Webpack，会有之后的学习记录。

在使用脚手架创建一个应用时，默认情况下Webpack相关配置都被隐藏起来了。

配置可以通过`"eject": "react-scripts eject"`暴露出来。执行`yarn eject`即可，这个操作是不可逆的。

```html
02_learning_scaffold
├── config
│   ├── env.js
│   ├── getHttpsConfig.js
│   ├── jest
│   ├── modules.js
│   ├── paths.js
│   ├── pnpTs.js
│   ├── webpack.config.js
│   └── webpackDevServer.config.js
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── README.md
├── scripts
│   ├── build.js
│   ├── start.js
│   └── test.js
├── src
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   ├── logo.svg
│   ├── reportWebVitals.js
│   └── setupTests.js
└── yarn.lock
```

然后我们可以看到，项目中多了`/scripts`和`/config`两个目录，以及`package.json`和`yarn.lock`文件的变动。





