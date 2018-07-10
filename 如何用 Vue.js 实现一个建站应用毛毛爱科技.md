---
title: 如何用 Vue.js 实现一个建站应用
date: 2017-11-13 12:08:32
categories: "开发"
tags:
	- GitHub
	- JSON
	- HTML
	- 数据结构
	- QQ空间 

---

> 随着互联网飞速发展，越来越多的传统服务搬到了线上，商家急需一个官网介绍自己的产品，提高知名度。因此 “建站” 成为了一种刚需。本文就如何用 Vue.js 实现一个建站应用提供了解决思路。

作为前端工程师，相信大家都写过不少网站和应用，我把网站简单的分为 “表现型” 和 “操作型”。表现型可以是一个产品介绍网站，而操作型的典型代表是管理后台。不久前有机会参与一个建站项目的设计与开发，它属于操作类型网站但需要更深一层的抽象，它是一个 “创建网站的网站”。本文尝试基于 Vue.js 框架设计实现这样一个应用，用户通过拖拽模块就可以建站。希望通过本文的介绍，能够带给大家不一样的视角。

需求

获取需求是开始项目的第一步。一般来说建站工具大多提供以下功能：

 *  提供模板
 *  主题色
 *  丰富的功能模块，如图文、轮播、相册等
 *  模块可以拖拽以及根据需求配置
 *  支持创建多个页面的网站
 *  页面可以分栏分区，有一些布局上的变化
 *  网站支持手持设备

需求分析

我们在开始动手之前可以先分析一下需求。

从需求中可以提取到几个关键词：“模板”、“主题色”、“模块”、“页面”、“分栏” 。明确这些关键词的意义将有助于我们接下来的设计。

 *  页面：一个网站由一个或多个页面（Page）组成。
 *  分栏/分区：页面由不同的功能区组成，比如公司介绍、成功案例、联系我们等。它们可能是纵向排列的，也可能左右分栏排布，我们取个更恰当的名字 “区块”（Section）。
 *  模块：每个区块包含一个或多个组件，这些组件组合起来达到同一个目的。例如公司介绍这个区块可以用一段文字模块介绍公司大体情况，用一个轮播模块展示公司主打产品。这里所说的模块和我们熟悉的 “组件”（Component）划等号，是我们要实现的最小功能单元。
 *  模板：模板可以有很多种定义。此处我们可以理解为一个页面的布局，类似于 QQ 空间可以选择的分栏布局。模板定义了一个页面包含的区块**数量**和每个区块的**横向占比**。
 *  主题色：每个网站都应有自己的风格。可以简单认为：风格 = 模板 + 主题色，不同的模板搭配不同的主题色形成了不同风格的网站。

网站的数据表示

那么问题来了，我们应该如何存储我们的网站呢？换句话说，存在数据库里面的是一个怎样的数据结构？是一个包含 HTML 的超大字符串吗？不急，基于上面的关键词定义我们可以总结一下：

1.  一个网站包含几个页面，一个页面包含几个区块，一个区块包含几个模块。可以看出这是一个树形结构，见下图。

![如何用 Vue.js 实现一个建站应用][Vue.js]

用 JSON 格式可以把它表示成：

    { "id": 1, "name": "xxx公司" "type": "site", "children": [{ "type": "page", "name": "首页", "children": [{ "type": "section", "name": "公司简介", "children": [{ "type": "paragraph" }, { "type": "carousel" }] }] }] }

1.  那么模块就是树中的叶子节点，需求中要求模块可以配置，我们可以把配置分为两部分：包含的内容（content）和设置（config）。举例来说，轮播模块中 content 存放的是几张图片的 URL，config 可以是轮播切换的动画效果、是否开启自动播放等设置。
2.  与模块一样，site、page、section 都是树中的节点，都可以根据需要在节点上增加 content 和 config。只不过对于这几类节点来说 content 其实就是 children，只有 config 属性。
3.  主题色是网站节点的配置项，可以在 `site.config` 中增加 `themeColor` 属性来表示。
4.  如何支持手持设备呢？前面说到模板定义了板块的横向占比，所以在板块的 config 属性中可以配置该板块在不同尺寸的横向占比。若采用 Bootstrap 的 12 栏栅格系统的话，可以很方便的通过设置 class 来达到目的。例如某个板块的 config 中 `class="col-xs-12 col-sm-6 col-md-3"` 表示该板块在手机下横向占 100%、平板占 50%、PC 占 25%。

![如何用 Vue.js 实现一个建站应用][Vue.js 1]

综上，一个网站可以完整的表示为一个树形 JSON。该树中包含了所有页面、板块、模块的内容和配置。

从数据到网站

我们已经有了网站的数据表示，那么下一个问题是如何从数据中渲染出网站呈现给用户呢？其实我们只要想办法渲染这棵 JSON 树就行了。

两步走：

1.  编写每个节点的代码，每个节点接受 `node` 属性和 `themeColor`
2.  遍历 JSON 树，在对应位置渲染对应的节点（即父子节点的包含关系）

第一步是个 “体力活”，此处以单段文字模块为例：

    <!-- Paragraph.vue --> <template> <div> <h1 :style="{color: themeColor}">{{node.content.title}}</h1> <small v-if="node.config.showSubTitle">{{node.content.subTitle}}</small> <p>{{node.content.detail}}</p> </div> </template> <script> export default { name: 'paragraph', props: ['node', 'themeColor'] } </script>

完成所有节点代码编写之后，第二步，我们需要写一个类似于 “renderer” 的组件来递归的渲染 JSON 树。基本思路是该组件先渲染自己，然后渲染自己的后代，每个后代也重复此渲染过程，如此渲染整棵树。

这里需要根据节点的 `type` 属性也就是一个 String 来获取对应的组件定义。幸运的是 Vue.js 中已经有这样的动态组件 `Component` ，此组件的 `is` 属性接受一个 String。由此我们的 render 组件可以这样写：

    <!-- render.vue --> <tempplate> <component :is="node.type" :node="node" :theme="themeColor"> <render v-for="child in node.children" :key="child.id" :node="child" :theme="themeColor" /> </component> </tempplate> <script> // 导入JSON 树中所涉及的所有节点 import Page from './Page.vue' import Section from './Section.vue' import Paragraph from './Paragraph.vue' export default { name: 'render', props: ['node', 'themeColor'], components: { Page, Section, Paragraph } } </script>

注：若 Vue.js 没有提供动态 Component 组件，我们也可以利用 Vue.js 中的 `createElement` 方法自己实现该组件，详见此 gist（ gist.github.com/github-libr… ）。

至此，我们已经设计了网站的数据表示，以及从数据到页面的渲染。那么这棵 JSON 树从何而来呢？

编辑与保存

要创建一个网站，用户在后台会经历选择样板站、调整色调、拖拽模块、编辑模块内容和配置、保存等操作。值得注意的是，此处用户选择的样板站跟文章开头的模板定义有些差别。如果说模板是网站的骨架的话，那样板站是填充了初始数据（默认色调、板块中包含的模块以及模块的默认内容）的模板。

从数据的角度来看，可以更清楚地看到这些步骤是如何逐步生成这棵树的。

<table>
 <thead>
  <tr>
   <th>界面操作</th>
   <th>影响数据</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>选择模板（样板站）</td>
   <td>该模板定义的初始树，包含默认的色调和模块</td>
  </tr>
  <tr>
   <td>选择色调</td>
   <td>更新 <code>site.config.themeColor</code></td>
  </tr>
  <tr>
   <td>拖拽模块到区域中</td>
   <td>在对应的 <code>section.children</code> 的数组中 push 一个组件节点</td>
  </tr>
  <tr>
   <td>在区域中排序模块</td>
   <td>在对应的 <code>section.children</code> 的数组中重新设置组件节点的 index</td>
  </tr>
  <tr>
   <td>编辑模块内容和配置</td>
   <td>更新对应模块的 content 和 config 中的属性</td>
  </tr>
  <tr>
   <td>保存网站</td>
   <td>把 JSON 树存入数据库持久化</td>
  </tr>
 </tbody>
</table>

既然选择了 Vue.js，我们可以选择官方推荐的 Vuex（ vuex.vuejs.org/en/ ）来管理状态。

首先创建一个 Vuex 实例，该实例包含一个 site 对象和一些对节点的操作：

    // store.js import Vue from 'vue' import Vuex from 'vuex' Vue.use(Vuex) const store = new Vuex.Store({ state: { site: {} }, mutations: { changeThemeColor () {}, addModule () {}, sortModule () {}, removeModule () {}, updateModule () {} }, actions: { getSite () {}, saveSite () {} } })

理论上有了这些方法我们已经可以从通过**程序**更新这棵树了，但作为编辑后台，我们还要提供一些界面上的入口让用户来编辑这棵树。

也就是说，我们需要**在编辑后台渲染一个可以编辑的网站，在网站上线后渲染一个只读的网站**，即同一个 JSON 树需要渲染两次。因为 renderer 是根据节点的 type 来渲染对应的组件的，所以对于编辑后台我们需要给每个节点取另一个 type，比如统一加一个前缀 `edit-`。例如 Paragraph 这个组件的编辑组件可以编写如下：

    <!-- EditParagraph.vue --> <template> <edit-wrapper> <paragraph :node="node" :themeColor="themeColor" /> </edit-wrapper> </template> <script> // EditWrapper提供统一的编辑入口，内部仍需要渲染一次 Paragraph 便于实时预览编辑结果 import EditWrapper from './EditWrapper.vue' import Paragraph from './Paragraph.vue' import { mapMutations } from 'vuex' export default { name: 'edit-paragraph', props: ['node', 'themeColor'], methods: { ...mapMutations(['updateModule']) } } </script>

用户可以通过这个组件的界面入口编辑 Paragraph 这个模块（此处略去 `edit-wrapper` 的实现，事实上组件的编辑可以通过弹窗加表单实现，也可以是更方便的 inline editing ）。

如此一来，每个模块都有一个对应的编辑模块，在两次渲染的时候便存在一个转换的过程。假设在数据库中存的是只读的树，那么在编辑后台获取到该树时需要转换成可编辑树从而渲染成带有编辑入口的网站，在保存时则需要转换成只读树保存。转换的过程其实很简单，把每个节点的 type 属性增加或删除前缀即可。

拖拽功能

我们选用拖拽的方式在区域中加入模块以及排序模块。有很多开源的库帮我们做了拖拽的实现，甚至有 Vue.js 的封装。此处推荐 `Vue.Draggable`（ github.com/SortableJS/… ）这个库，它是基于 Sortable.js 做的一层封装。其典型的应用场景如下：

    <draggable v-model="myArray" :options="{group:'people'}" @start="drag=true" @end="drag=false"> <div v-for="element in myArray">{{element.name}}</div> </draggable>

在我们的场景中，只需在 `draggable` 组件上监听 `add` 和 `sort` 事件调用 store 中对应的方法即可。

如本节表格所述，拖拽只是界面上的操作方式，本质上是对数组中元素的增加和调整 index 的操作。

变更检测

为了及时保存用户的编辑，我们可以在用户修改主题色、编辑模块、删除模块等操作时自动保存。本质上是需要检测 store 中 site 这个状态的变更。Vuex 中的插件（plugin）可以针对改动做一些类如记录日志和持久化的操作，我们可以写一个 autoSave 的插件来实现。

    // store.js const autoSave = (store) => { store.watch( state => state.site, (newV, oldV) => { store.dispatch('saveSite') }, { deep: true } ) } const store = new Vuex.Store({ state: { site: {} }, plugins: [autoSave] })

至此，我们已经用 Vue.js 和相关技术实现了文章开头列出的建站应用的需求。

结语

本文从需求分析、方案设计、编码实现分别介绍了用 Vue.js 写一个建站应用可能会遇到的问题以及大致的思路。可以看出，文章很大篇幅都在讲数据，包括数据格式的设计、数据的操作、数据变更的检测。这是因为 Vue.js 框架帮我们做了从数据到界面的渲染以及数据变更后界面的更新的工作，我们要做的是管理好这些数据。Vue.js 框架分担了我们的工作，提高了开发效率，使得我们可以专注于业务逻辑设计，这也是框架价值的体现。


[Vue.js]: /pro/os/crawler/MFAB-BJ6V-NBUN.jpg
[Vue.js 1]: /pro/os/crawler/U7BJ-I2ZJ-BMNJ.jpg
 *  **原文作者：** 毛毛爱科技
 *  **原文链接：** https://www.toutiao.com/item/6486737539112632846/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。