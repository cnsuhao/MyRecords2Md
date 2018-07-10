---
title: Vue2.0 实践，顺手撸了一个小项目
date: 2017-03-27 00:12:12
categories: "开发"
tags:
	- Pages
	- 多看阅读
	- 路由器
	- GitHub
	- 软件

---

传送门：https://github.com/icepy/index-oa-template，或阅读原文。

这个项目的背景还要从“移动企业门户”说起，这是我厂的一个小项目，现也开源在Github上，其目的是为了给企业开发自己的企业门户提供参考和模板，可以快速的用起来，或者参考一下我们是如何来实现移动企业门户的。

Demo 效果如图：

![Vue2.0 实践，顺手撸了一个小项目][Vue2.0]

这个项目，主要使用了Vue2.0和Vue-router，其实Vue-router不是必须的，因为这只有一个页面，但是也应用到了Vue的方方面面。在创建项目时，用了vue-cil来初始化这个项目，不过我也为它修改了一些自己想要的东西，没错就是weex相关的构建与入口，具体如何用同一个Vue2.0项目既可以跑Web也可以跑Native，你可以参考一下我这个项目中的weex分支。

既然用到了vue-router，那还是简单的贴一下代码，非常简单：

importVuefrom'vue'

importRouterfrom'vue-router'

importHomefrom'pages/home/index.vue'

Vue.use(Router);

constroutes= \[

\{

path:'/',

name:'home',

component: Home

\}

\];

exportdefaultnewRouter(\{

routes: routes

\});

importVuefrom'vue';

importAppfrom'./App';

importrouterfrom'./router';

newVue(\{

el:'\#app',

router,

template:'<App/>',

components: \{ App \}

\});

router的配置可以看的出来，非常的简单，一个对象用来描述一个path的所有，也是如此，你可以顺手的描述其它的规则，有path，component，等等。

整体的组件也是非常简单的，因为只需要引用即可。实际上稍微复杂一点的地方，主要在page/home/index.vue文件中，因为在这个文件里做了一些其它的事情，比如获取userid实现免登，那么只有当获取到userid之后才能去获取用户信息，也就是界面 下午好，管理员，icepy的变更。我用了$watch来处理这个问题，比较简单，：

this.$watch('userId',function()\{

this.getUserInfo();

\});

getUserInfo定义在methods中：

getUserInfo:function()\{

// 根据userid获取用户详细信息

constself=this;

constgetUserInfoRequest= \{

url:OPENAPIHOST+'/api/get',

params: \{

userid:this.userId

\}

\};

dingWISDK.getUserInfo(getUserInfoRequest).then(function(response)\{

constdata=response.data;

self.meta.userInfo= data;

\}).catch(function(error)\{

alert('获取用户信息 error：'+JSON.stringify(error));

\});

\}

当userid有变化时，立即调用getUserInfo来更新用户界面。

其实从源代码中可以看见，界面都是一个个单独的组件，通过数据的传递来渲染，单组件文件系统，没什么好说的，大家有兴趣可以多看一看官网的文档。当你不需要加入vuex时，对于驱动界面还是比较简单的，书写下来，只是有一些地方需要注意，特别是React开发者转过来的：

 *  props传递，需要用v-bind:，并且在子组件中用props:\[\]来声明
 *  监听事件时不需要bind，v-on:click="microAppsOpenLink(item,$event)"
 *  建议很多东西都写全，不要写简写，比如:xxx这种，如果不是长期应用vue的开发者，看起来还要思考很久
 *  因为dom变成了模板，模板有编译的过程，处理类似比如自动触发一个事件这样的需求时需要想一想，ref的处理，看起来没那么直观
 *  引用组件的时候，需要显示的声明，比如：

components: \{

banner: banner,

applist: applist,

item: item,

admin: admin,

userlist: userlist,

appmanager: appmanager

\},

也许还有很多需要注意的细微之处，待你慢慢挖掘了。

比较好的消息是WebStorm开始原生支持Vue了，可见其火热的趋势，回过头来可以看到我们做事情时的一些反思：贵在坚持。

你身边如果有朋友对混合领域（跨技术栈）或全栈，编程感悟感兴趣，可以转发给他们看哦，^\_^先谢过啦。

更多精彩内容可关注我的个人微信公众号：搜索**fed-talk**或者扫描下列二维码，也欢迎您将它分享给自己的朋友。


[Vue2.0]: /pro/os/crawler/RRMR-ZI2Q-VUVQ.jpg
 *  **原文作者：** 象尘说
 *  **原文链接：** https://www.toutiao.com/item/6401840877588709889/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。