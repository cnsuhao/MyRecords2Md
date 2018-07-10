---
title: 为什么要使用JavaScript(中文：重思JavaScript的好与坏)中英对比版本
date: 2012-12-25 18:15:21
categories: "开发"
tags:
	- Java

---

I was among many developers that recently rediscovered JavaScript. Indeed as a very capable language. But after using *the new* JavaScript for a solid year, I came to the conclusion that JavaScript *still* sucks. And no, I'm not talking about its wacky comparison operators. There is a deeper problem, and a solveable one at that. Let me explain.

![monkeys.jpg][]

## No Nonsense ##

I must've been 13 when my dad gave me [my first programming book][]. I was overjoyed to find that under the hood, computers were not a tangled mess of wires and clockwork, no, the code was clear as day. I felt like I knew it all along, and in a big way I did. After all, what is code but a formalization of rational human thought?

Now, in all honesty I was expecting a Java book. I googled something along the lines of "is JavaScript and Java the same language". Turned out they're not, and so, Java ended up being my second. I didn't even get through the introduction before the first problem appeared: **my mom and stepdad didn't want me installing software on the family computer—I was left without a Java compiler**. I remember double checking my code before uploading it to [an online compiler][], then waiting, downloading the jar... rinse, repeat. Boy was it tedious.

From my perspective back then, JavaScript was truly a no-nonsense language. You whip up an HTML file in Notepad, open it in Internet Explorer and you're set. There were no classes, no private and public methods, no final variables. I didn't know the word "overengineered" back then, but it would have described exactly my impression of Java. So:

1.  No compilation
2.  Ubiquitous platform
3.  No advanced language features
4.  Oh, almost forgot: **View Source**!

Why would a great language like that be so hated?

## Ad-hoc Hell ##

Here's the problem: JavaScript programs inherited from their language a complete lack of design guidance. **Everything was ad-hoc:**

1.  **No OOP**. I am not familiar with any other way to structure *imperative* programs. Yes, JavaScript has a rudimentary object model, but literally no one knew how it actually worked.
2.  **No standard library**. Copying functions from forums and pasting them into your code. Not fun.
3.  **No module system**. Constant scouting for download links and ridiculously long source files.
4.  **No language resources**. No high-quality resources (and hardly a single decent book) meant your best reference was misguided blog posts and newbie forum questions.

So, what happened to make JavaScript popular again?

## Back in Vogue ##

I think what jump-started JavaScript's reemergence was Google's work on the V8 engine. Suddenly, JavaScript was fast. *Real fast*. Fast enough that people decided to bite the bullet and use it to write *web applications*.

Then [Node.js][] came out with some wanky benchmarks and, more importantly, a promise for code reuse on the client and server, a promise not really capitalized on until recently, with the likes of [Meteor][] and[Derby][]. Things started to change for the better:

1.  **Download buttons got BIGGER**
2.  Node.js kind of sort of standardized an import system
3.  People started writing pretty good libraries
4.  [Good resources][] started popping up

However:

1.  **Still no OOP**
2.  Client and server JavaScript still completely disconnected
3.  Documentation, even for high profile projects, is lousy at best

## Why, why? ##

I think the root of the problem with modern JavaScript and the main reason that the language still sucks lies in a certain sentiment of the JavaScript community, the sentiment that **JavaScript was a good language all along, it was just a misunderstood language**. Um, no. JavaScript was a premature bastard baby, raised by frickin' wolves. Stop defending JavaScript as a language and start embracing JavaScript as a *platform*, and as a *compilation target*.

### Documentation ###

People can laugh at Java all they want but when I look at Java documentation I see *exactly* which objects I can make, *exactly* which parameters each method takes, *exactly* which errors they throw. The best I could hope for with a JavaScript library is a fancy piece of example code. This makes it nearly impossible to write *correct* software, unless you read the source code of the library you are using. Even such integral JavaScript projects as [Express][] have lousy API documentation. ([Ember's][Ember_s] excellent API documentation might be a counter-example.)

### Syntax Sugar ###

The JavaScript community is too quick to dismiss syntax sugar. I think **syntax sugar creates expressiveness**. The same way you abstract repeating logic in your code, your language ought to abstract common idioms.

1.  It makes the code more concise.
2.  **It helps communicate the *meaning* behind your code rather than the algorithm.** I already know the array iteration algorithm, thank you very much. Stop repeating it and just tell me what you are trying to do.

As a coder, your main audience isn't your computer, it is other programmers, including your future self. Yes, we can all feel cool writing tedious for loops, but who would honestly prefer reading `for (var i=0; i<arr.length; i++)` rather than `for e in arr`? As a side note, the JavaScript version of this loop is actually slower than its CoffeeScript counterpart, do you know why?

### JSLint ###

As if being a JavaScript developer wasn't painful in itself, [JSLint][] comes and adds insult to injury. I imagine linting JavaScript as an embarrassing punishment in some kind of coding boot camp, akin to cleaning the toilet with a tooth brush. In fact, I think I would literally shave a yak instead.

On a more serious note, as programmers we should be looking for automation. Don't you wish you could automate JSLint? Oh wait, you can! CoffeeScript generates perfectly linted code, which I find quite hilarious. Props to [Jeremy][] for that one.

![yak.jpg][]

### OOP ###

JavaScript's object model is not enough. Prototypal inheritance is a low level feature that *can be used* to create a meaningful object model, but in no way constitutes one by itself.

This is part of the reason that JavaScript's API documentation sucks so badly, how are you supposed to document your object-oriented code when your language doesn't even have classes? How are JavaScript libraries, and, most importantly, JavaScript developers supposed to interoperate when [we don't even agree on how to instantiate objects][we don_t even agree on how to instantiate objects]?

## What Are the Options? ##

Years have passed, laptops were replaced numerously, regimes crumbled, subatomic particles were discovered. JavaScript is still the same. How many years are you willing to wait for basic features? Following the pessimistic tone of this article, let's first list the things that *aren't* going to be an option:

1.  Mozilla's own JavaScript [1.7][] or [1.8][]. The former was released 6 and half years ago, but being backwards-incompatible, literally no one used it and no one ever will, same goes for the latter.**Both dead on arrival.**
2.  ECMAScript Harmony. The most recent [Design by Committee][] sloth, an evolution of the above. Transitioning to this new ECMAScript is going to be an excruciatingly slow and painful process, especially since some of its features are [incompatible with JavaScript][]. **Not a solution unless you're willing to wait for a decade.**

And, finally, the viable options:

1.  **[CoffeeScript][]**. The syntax is very nice and it supports classes. I think CoffeeScript is not the end, but it is a spectacular start.
2.  **[TypeScript][]**. Builds on JavaScript to add optional static typing, which I, personally, am a big fan of.Not enough to replace JavaScript, but a good start as well. As pointed out by [Mohamed Mansour][]and Dave Hodder in the comments, TypeScript goes further by implementing some ECMAScript proposals and perhaps ECMAScript will eventually borrow the optional static typing from TypeScript. Very interesting.
3.  **[Google Dart][]**. A wholly new language that ships with a to-JavaScript compiler and [some JavaScript interop][] but overall is not meant to integrate into the JavaScript ecosystem. I fail to see a compelling reason for this other than some Google engineers ignoring reality in favor of making a cool new language from scratch. As a coder I can understand that, but I think it will prove to be the language's downfall.
4.  **[HaXe][]**. Thanks to Laurence Taylor for pointing it out in the comments. This is a high-level language that compiles to JavaScript, Flash, NekoVM, PHP, C++, C\# and Java (soon). I've never used it myself, but it looks really impressive.

### Conclusion ###

One thing I am ready to bet on: the winner will be a [compile-to-JS][] language. We aren't ready for a transition yet, but we are making progress and even though many efforts diverge, they all pave the way for the future of the web. Regardless of which language wins in the end, I want to emphasize my main point: **JavaScript as it stands today is painfully inadequate, we need to embrace the need for a new language and push forward!**

Props to all the people making this happen and here's to hoping that [more Node developers divorce their JavaScript purism][] and join the ranks.

\---------------------------------------------------------------------

from:http://boronine.com/2012/12/14/Why-JavaScript-Still-Sucks/

最近，我和许多程序员一样，对JavaScript进行重新探讨。事实上，JavaScript是一个非常有能力的语言，但使用新JavaScript一年后，我得出的结论却是JavaScript仍然很烂。我并不是在讨论其古怪的比较运算。而是从更深的层次去思考。与此同时，还会提供一些解决方案供你选择。


[![115029kwcdc44s4d4t222u.jpg][]][115029kwcdc44s4d4t222u.jpg]


直奔主题


 13岁时，父亲送给我第一本编程书籍——JavaScript初学者编程。让我兴奋的是，我发现电脑并不单单是由电线和发条所组成，里面的代码清晰明了。我觉得我基本上理解它了，实际上也差不多。终究，代码除了人类理性思想的形式化还能是什么？


 说实话，我当时希望遇到的是Java，我甚至在谷歌上搜索：“JavaScript和Java是同一种语言吗？”当然不是。��以Java成为了我[学习][Link 1]的第二门语言，不过我母亲和继父不希望在家庭电脑里安装软件，所以我无法安装Java编译器。我清晰地记得，在把代码上传到一个[在线编译器][an online compiler]之前，得反复检查代码，然后等待、下载jar包……


 我当时认为，JavaScript是一个非常严肃的语言。在Notepad里新建一个HTML文件，然后在IE里打开，就这样开始了。没有类、没有私有和公有方法，没有final变量。当时，我甚至不知道什么叫“过度设计”，但它准确地描述了我印象中的Java：


 *  无需编译
 *  无处不在的开发平台
 *  没有高级语言特性
 *  查看源文件


为什么这么伟大的语言还会讨人厌？


Ad-hoc地狱


 下面是一些问题：设计之匆忙、初衷仅仅实现简单的网页互动、JavaScript继承完全缺乏设计指导，一切都是ad-hoc：


1.  没有OOP：JavaScript确实存在基本的对象模型，但几乎无人知道它是如何工作的。
2.  没有标准库：从[论坛][Link 2]上复制粘贴代码不好玩。
3.  没有模块系统：不断查找下载链接和很长的源文件。
4.  缺乏语言资源：没有高品质的资源（几乎没有一本像样的书籍）意味着你只能从论坛或博客上获得一些非官方的参考。


既然JavaScript有这么多缺陷，到底是什么让它如此流行？


复兴


 JavaScript崛起的背后可能与谷歌V8引擎��息相关。JavaScript突然变得很快，真正地快速。所以人们开始硬着头皮使用它来编写Web[应用][Link 3]程序。


 然而，[Node.js][]推出了一些更吸引人的基准，允许代码在客户端和服务器端重用，很像[Meteor][]和[Derby][]。因此，情况开始好转：


1.  下载按钮变得更大
2.  Node.js kind of sort of standardized an import system
3.  人们可以编写更好的库
4.  [优秀的资源][Good resources]开始出现


然而：


1.  仍然没有采用OOP
2.  客户端和服务端的JavaScript仍然没有关联
3.  文档甚至是高知名度项目文档也很糟糕


为什么？


 我认为现代JavaScript仍然较烂的根源是其处在一个特定的社区氛围中，该氛围一直强调：JavaScript是一门非常好的语言，但却一直被人误解。不，JavaScript只是一个提早诞生且没有先例可循的语言。停止包庇并且开始把JavaScript作为一个平台去拥抱，作为一个编译目���。


文档


 人们可以尽情嘲笑Java，但当我看Java文档时，我可以非常明确地知道哪个对象可以创建、每个方法里的参数、需要抛出的异常等。最好的事情莫过于使用JavaScript库作为精选示例代码。这几乎是不可能写出正确的软件，除非你阅读库源码。甚至连如此完整的JavaScript项目[Express][]也存在令人生厌的API文档（[Ember][Ember_s]他们那些非常优秀的API文档可能是反例）。


语法���


 JavaScript社区很快就抛弃了语法糖。语法糖是极富创造力的。将重复逻辑代码抽象出来，可以通用，帮助程序员以更好的方式编写代码。


 *  让代码更加简洁
 *  有助于表达代码背后的意思，而不是算法。我已经知道了数组迭代算法，好吧，谢谢你，但是请你不要告诉我这些，我只想知道你要做的东西。


 作为一名编码人员，你的主要观众不是电脑，而是其他程序员，也包括将来的自己。书写冗长的for循环很爽，但说实话，for (var i=0; i<arr.length; i++)与for="" e="" in="" arr="" 比起来，人们更愿意读后者。顺便说一下，这里的javascript版的循环比coffeescript速率要慢，你知道为什么吗？<="" font="" style="word-wrap: break-word;">


JSLint


 如果程序员使用JavaScript尚未感到痛苦，那[JSLint][]成功做到了这点，而且非常“成功”！ 想象一下，在某个编码的新兵训练营里，���要对JavaScript进行尴尬的惩罚，然后在其身上进行拔毛，这好比是拿着牙刷在刷厕所。事实上，我宁愿从牦牛身上拔毛。这是原文中的例子，小编的理解是JSLint的出现是给JavaScript挑刺的，而这样还不如直接在JavaScript身上拔毛。


[![115033goo9w0gx7bxr9ooa.jpg][]][115033goo9w0gx7bxr9ooa.jpg]


 更值得注意地是，作为程序员，我们应该寻找自动化，难道你不希望自己可以自动化JSLint吗？当然，���可以。CoffeeScript可以完美的生成linted代码，正如[Jeremy][]。


OOP


 JavaScript对象模型还是不够成熟，原型（Prototype）继承是一个低级别的功能，可以创建一些非常有意义的对象模型，但却无法基于本身进行构建。


 这也可能是JavaScript API为什么会那么糟糕的原因，当你所使用的语言甚至没有类时，该如何记录面向对象代码呢？ 最重要的是，当[我们甚至不知道实例化对象][we don_t even agree on how to instantiate objects]时，JavaScript开发人员该如何实行互操作？


有什么可选项吗？

 年复一年，笔记本逐步成为主流、政坛也发生了不少的变化，而JavaScript仍然保持不变。你愿意等一些基本特性多少年？在这篇悲观论调的文章里，让我们先看看下面这个列表：


1.  Mozilla发布了自己JavaScript[1.7][]或[1.8][]，前者是在6年半之前发布的，且不向后兼容，但并无人使用且永远不会有，同样也适用于后者。
2.  ECMAScript Harmony。最近，由于[设计委员会][Design by Committee]的原因，发布新版ECMAScript将会有一个极其缓慢和痛苦的过渡过程，特别是它有一些特效是与[JavaScript不兼容][incompatible with JavaScript]的。没有解决方案，除非你愿意再等数十年。


下面列出一些可选项


1.  [CoffeeScript][]语法非常漂亮且支持类。我认为CoffeeScript并未结束，只不过是刚刚华丽���开启。
2.  [TypeScript][]一种编译到JavaScript的语言，可以在JavaScript中构建并且添加了静态类型。个人而言，我是它的大粉丝。正如[Mohamed Mansour][]和Dave Hodder在评论中指出的那样，TypeScript正在进一步实施ECMAScript的一些建议，也许ECMAScript最终会从TypeScript中借鉴可选的静态类型。
3.  [Google Dart][]一个全新的语言，附带JavaScript编译器和一些[JavaScript互操作][some JavaScript interop]，但整体���并未整合到JavaScript生态系统中。
4.  [HaXe][]这是一个高级语言，可以编译JavaScript、Flash、NekoVM、PHP、C++、C\#和Java（不久后），虽然我从未使用过，但它真的令人印象深刻。


总结


 最后，我敢押注，最终的获胜者肯定是编译为JavaScript的语言。虽然我们还没真正准备好这样的过渡，但是所取得的进展仍是有目共睹的。尽管也存在些偏离，但他们正在替未来的Web发展铺平道路。���管最终哪门语言会胜出，我都想强调一下我的主要观点：JavaScript目前仍然存在许多不足，我们需要拥抱一个新语言并且推进它。

\-----------------------------------------------------------

from:http://www.html5cn.org/article-4332-1.html



[monkeys.jpg]: http://boronine.com/img/monkeys.jpg
[my first programming book]: http://www.amazon.com/JavaScript-Programming-Absolute-Beginner-Harris/dp/0761534105
[an online compiler]: http://www.innovation.ch/java/java_compile.html
[Node.js]: http://nodejs.org/
[Meteor]: http://meteor.com/
[Derby]: http://derbyjs.com/
[Good resources]: https://developer.mozilla.org/en-US/
[Express]: http://expressjs.com/api.html
[Ember_s]: http://emberjs.com/api/
[JSLint]: http://www.jslint.com/
[Jeremy]: https://github.com/jashkenas
[yak.jpg]: http://boronine.com/img/yak.jpg
[we don_t even agree on how to instantiate objects]: http://stackoverflow.com/questions/383402/is-javascript-s-new-keyword-considered-harmful
[1.7]: https://developer.mozilla.org/en-US/docs/JavaScript/New_in_JavaScript/1.7
[1.8]: https://developer.mozilla.org/en-US/docs/JavaScript/New_in_JavaScript/1.8
[Design by Committee]: http://en.wikipedia.org/wiki/Design_by_committee
[incompatible with JavaScript]: http://stackoverflow.com/a/6510903/212584
[CoffeeScript]: http://coffeescript.org/
[TypeScript]: http://www.typescriptlang.org/
[Mohamed Mansour]: https://twitter.com/mohamedmansour
[Google Dart]: http://www.dartlang.org/
[some JavaScript interop]: http://www.dartlang.org/articles/js-dart-interop/
[HaXe]: http://haxe.org/
[compile-to-JS]: https://github.com/jashkenas/coffee-script/wiki/List-of-languages-that-compile-to-JS
[more Node developers divorce their JavaScript purism]: http://procbits.com/2012/05/18/why-do-all-the-great-node-js-developers-hate-coffeescript
[115029kwcdc44s4d4t222u.jpg]: http://www.html5cn.org/data/attachment/portal/201212/25/115029kwcdc44s4d4t222u.jpg
[Link 1]: http://www.html5cn.org/portal.php?mod=list&catid=13
[Link 2]: http://www.html5cn.org/forum.php
[Link 3]: http://www.html5cn.org/portal.php?mod=list&catid=20
[115033goo9w0gx7bxr9ooa.jpg]: http://www.html5cn.org/data/attachment/portal/201212/25/115033goo9w0gx7bxr9ooa.jpg