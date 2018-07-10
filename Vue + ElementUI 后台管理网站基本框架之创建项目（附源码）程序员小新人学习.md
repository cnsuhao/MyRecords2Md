---
title: Vue + ElementUI 后台管理网站基本框架之创建项目（附源码）
date: 2018-06-11 10:25:40
categories: "开发"
tags:
	- CSS
	- jQuery
	- 技术
	- 路由器

---

 *  创建工程目录
 *  ajax插件选择
 *  模拟ajax返回数据
 *  跨域问题
 *  优化webpack配置
 *  增加babel-polyfill
 *  增加路径别名
 *  修改单页面应用根模板index.html位置
 *  修改chunk文件名
 *  完善各种类型的css-loader
 *  NEXT——路由篇
 *  源码
 *  ���系列目录

# **创建工程目录** #

--------------------

在开始一切的讲解前，我们先创建一个全新的工程。推荐直接使用vue官方提供的命令行工具vue-cli，它能快速的生成一个基于vue和webpack的单页面应用。

``````````
# 全局安装vue-clinpm install vue-cli -g# 选择你的项目根文件夹# e:# cd vue-project# 创建webpack项目# vue init [vue-cli模板名称] [项目文件夹]vue init webpack my-project# 安装依赖npm install12345678910111213
``````````

在安装中有自动提示，要求输入项目名称、描述、作者、关键字、是否使用eslint、是否加入单元测试。有需求的话按照提示直接输入或选择即可，没有需求的话一路按回车和Y就行了。

当完成上述步骤后，你就能够在e:ue-projectmy-project中找到自动生成完毕的项目。然后大家可以根据实际需要或者编写习惯来创建其中的具体目录。以下为参考目录：

``````````
|- src/| |- assets/ // 静态文件目录：css、images等| |- components/ // 组件文件目录| |- page/ // 具体业务页面目录| |- router/ // vue-router的路由目录| |- store/ // vuex目录| |- util/ // 工具类目录|- main.js // webpack入口文件|- App.vue // 入口页面123456789
``````````

# **ajax插件选择** #

--------------------

在jQuery时代，ajax的使用一般直接用jQuery自己提供的就好。但是当使用vue作为底层开发框架时，因很少再去直����作DOM的关系，jQuery本身就很少被提及了。所以这里推荐大家使用axios作为ajax插件，这也是vue官方所推荐的插件。axios的安装和使用都很简单，但其在项目中的具体配置会在今后的章节中涉及，此处不做详细讨论。现在我们只需要知道它能实现ajax就可以了。

# **模拟ajax返回数据** #

--------------------

在实际的开发过程中，经常会遇到前端需要后端的数据，但后端并没有完成��功��的情况。这种问题有很多种解决方案，比如搭建一个前后端公用的Mock Server。不过在项目前期如果没有充足准备的话，对于前端同学来说，使用Mockjs来实现这个需求则更加方便、快速一点，和项目本身集成到一起，不用再搭建服务器了。

注意：在本地项目中集成Mockjs，只应该用于前期开发或工作比较忙的时候，后期还是应该有统一的API测试平台，保证前后端都能通过该平台测试自己的功能，并能够对API协议进行统一管理。

# **跨域问题** #

--------------------

如果在开发过程中，ajax请求地址非本服务相同主机和相同端口，则会因浏览器自身的安全机制，导致出现跨域问题。解决该问题最好的方法是让后台同学配置CORS，或者在webpack中设置proxyTable，或者自己搭建nginx服务进行反向代理。

这里主要说一下关于webpack中proxyTable的设置（其实是vue-cli��成的��目自带的配置项）。请注意，webpack的proxyTable只在开发环境中有效！以下说明均引用自vue-cli对webpack模板的说明，这里为原文链接：vuejs-templates

> 在config/index.js中编辑dev.proxyTable项，以设置代理规则。在开发环境中，实际上使用的是http-proxy-middleware插件实现的代理功能，所以它的详细用法你应该参考其官方文档，这里有个简单的例子：

``````````
// config/index.jsmodule.exports = { // ... dev: { proxyTable: { // 所有以/api 为前缀的请求将被代理到http://jsonplaceholder.typicode.com // 即 /api/getNav -> http://jsonplaceholder.typicode.com/getNav '/api': { target: 'http://jsonplaceholder.typicode.com', changeOrigin: true, pathRewrite: { '^/api': '' } } } }}1234567891011121314151617
``````````

# **优化webpack配置** #

--------------------

vue-cli默认生成的项目工程虽然很全，但仍有一部分需要进行修改，以更好的适应实际开发。这里仅列举针对工程的全局修改，在实际开发中可能会有更多的修改，比如增加loader等

**增加babel-polyfill**

vue-cli默认生成的项目中自带了babel-plugin-transform-runtime，其保证了一定的浏览器兼容性。但其存在两个问题：

1.  异步加载组件时，会产生 polyfill 代码冗余
2.  不支持对全局函数与实例方法的 polyfill
3.  介于以上两种缺点，我们需要增加babel-polyfill，同时删除babel-plugin-transform-runtime

在命令行中输入以下命令

``````````
# 安装babel-polyfillnpm install babel-polyfill --save# 卸载babel-plugin-transform-runtimenpm uninstall babel-plugin-transform-runtime --save12345
``````````

修改文件.babelrc

``````````
"plugins": [ // "transform-runtime"],123
``````````

在入口文件main.js中引入babel-polyfill

``````````
import 'babel-polyfill'1
``````````

**增加路径别名**

在实际开发中，某些路径层级可能会很深，如果使用相对路径可能会有无数的../../。为解决这种问题，我们可以增加路径别名，以减少开发过程中路径的复杂性。

修改文件：webpack.base.conf.js

``````````
resolve: { extensions: ['.js', '.vue', '.json'], alias: { 'vue$': 'vue/dist/vue.esm.js', '@': resolve('src'), 'util': '@/util', 'asset': '@/asset' ... }}12345678910
``````````

**修改单页面应用根模板index.html位置**

webpack默认的根模板路径与src目录同级。但我更喜欢把根模���放置到src目录中。如果你也有像我这样的需求，那么按以下修改即可

``````````
// 开发环境：webpack.dev.conf.jsnew HtmlWebpackPlugin({ filename: 'index.html', // template: 'index.html', template: './src/index.html', inject: true})// 生产环境`webpack.prod.conf.js`new HtmlWebpackPlugin({ filename: config.build.index, // template: 'index.html', template: './src/index.html', ...}),123456789101112131415
``````````

**修改chunk文件名**

如果你不希望看到���种以数���开头的文件名的话，可以按照以下方式修改。如果你觉得hash太长，也可以限定其长度

``````````
// webpack.prod.conf.jsoutput: { path: config.build.assetsRoot, filename: utils.assetsPath('js/[name].[chunkhash].js'), // chunkFilename: utils.assetsPath('js/[id].[chunkhash].js') chunkFilename: utils.assetsPath('js/[name].[chunkhash:7].js')}1234567
``````````

**完善各种类型的css-loader**

在build/utils.js 中我们可以看到，vue-cli项目自动帮我们判断了需要哪些css的loader。如果你想以后不再轻易动package.json的话，你完全可以把这些loader都安装上。其中安装sass-loader前需要提前安装node-sass。安装less-loader前需要安装less。以下为安装sass-loader和less-loader

``````````
npm install node-sass sass-loader less less-loader --save-dev1
``````````

# **NEXT——路由篇** #

--------------------

项目创建及配置完毕后，我们将开始编写后台管理系统中���重要也是最���础的部分——路由及其权限。基于权限的页面跳转将会是重中之重。

# **源码** #

--------------------

当前源码地址：https://github.com/harsima/vue-backend

请注意，该源码会不断更新（因为工作很忙不能保证定期更新）

![Vue + ElementUI 后台管理网站基本框架之创建项目（附源码）][Vue _ ElementUI]


[Vue _ ElementUI]: /pro/os/crawler/6J2A-FQER-YBNZ.jpg
 *  **原文作者：** 程序员小新人��习
 *  **原文链接���** https://www.toutiao.com/item/6565647532016271880/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。