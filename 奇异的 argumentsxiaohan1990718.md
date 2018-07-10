---
title: 奇异的 arguments
date: 2012-10-27 18:19:53
categories: "开发"
tags:
	- 技术
	- javascript

---

 *  原标题：arguments: A JavaScript Oddity
 *  原作者： [Andrew Tetlaw][]
 *  原连接： http://www.sitepoint.com/blogs/2008/11/11/arguments-a-javascript-oddity/ 中，有个叫 format 的函数充分发挥了这一特性：

``````````
function format(string) {
  var args = arguments;
  var pattern = new RegExp("%([1-" + arguments.length + "])", "g");
  return String(string).replace(pattern, function(match, index) {
    return args[index];
  });
};
``````````

这个函数实现了模板替换，你可以在要动态替换的地方使用 %1 到 %9 标记，然后其余的参数就会依次替换这些地方。例如：

``````````
format("And the %1 want to know whose %2 you %3", "papers", "shirt", "wear");
``````````

上面的脚本就会返回

``````````
"And the papers want to know whose shirt you wear" 。
``````````

在这里需要注意的是，即便在 format 函数定义中，我们仅定义了个名为 string 的参数。而 Javascript 不管函数自身定义的参数数量，它都允许我们向一个函数传递任意数量的参数，并将这些参数值保存到被调用函数的 arguments 对象中。

### 转换成实际数组 ###

虽然 arguments 对象并不是真正意义上的 Javascript 数组，但是我们可以使用数组的 slice 方法将其转换成数组，类似下面的代码

``````````
var args = Array.prototype.slice.call(arguments);
``````````

这样，数组变量 args 包含了所有 arguments 对象包含的值。

## 创建预置参数的函数 ##

使用 arguments 对象能够简短我们编写的 Javascript 代码量。下面有个名为 makeFunc 的函数，它根据你提供的函数名称以及其他任意数目的参数，然后返回个匿名函数。此匿名函数被调用时，合并的原先被调用的参数，并交给指定的函数运行然后返回其返回值。

``````````
function makeFunc() {
  var args = Array.prototype.slice.call(arguments);
  var func = args.shift();
  return function() {
    return func.apply(null, args.concat(Array.prototype.slice.call(arguments)));
  };
}
``````````

makeFunc 的第一个参数指定需要调用的函数名称（是的，在这个简单的例子中没有错误检查），获取以后从 args 中删除。makeFunc 返回一个匿名函数，它使用函数对象的（Function Object）apply 方法调用指定的函数。

apply 方法的第一个参数指定了作用域，基本上的作用域是被调用的函数。不过这样在这个例子中看起来会有点复杂，所以我们将其设定成 null ；其第二个参数是个数组，它指定了其调用函数的参数。makeFunc 转换其自身的 arguments 并连接匿名函数的 arguments，然后传递到被调用的函数。

有种情况就是总是要有个输出的模板是相同的，为了节省每次是使用上面提到的 format 函数并指定重复的参数，我们可以使用 makeFunc 这个工具。它将返回一个匿名函数，并自动生成已经指定模板后的内容：

``````````
var majorTom = makeFunc(format, "This is Major Tom to ground control. I'm %1.");
``````````

你可以像这样重复指定 majorTom 函数：

``````````
majorTom("stepping through the door");
majorTom("floating in a most peculiar way");
``````````

那么当每次调用 majorTom 函数时，它都会使用第一个指定的参数填写已经指定的模板。例如上述的代码返回：

``````````
"This is Major Tom to ground control. I'm stepping through the door."
"This is Major Tom to ground control. I'm floating in a most peculiar way."
``````````

## 自引用的函数 ##

您可能会认为这很酷，先别急着高兴，后面还有个更大的惊喜。它（arguments）还有个其他非常有用的属性：callee 。arguments.callee 包含了当前调用函数的被引用对象。那么我们如何使用这玩意做些的事情？arguments.callee 是个非常有用的调用自身的匿名函数。

下面有个名为 repeat 的函数，它的参数需要个函数引用和两个数字。第一个数字表示运行的次数，而第二个函数定义运行的间隔时间（毫秒为单位）。下面是相关的代码：

``````````
function repeat(fn, times, delay) {
  return function() {
    if(times-- > 0) {
      fn.apply(null, arguments);
      var args = Array.prototype.slice.call(arguments);
      var self = arguments.callee;
      setTimeout(function(){self.apply(null,args)}, delay);
    }
  };
}
``````````

repeat 函数使用 arguments.callee 获得当前引用，保存到 self 变量后，返回个匿名函数重新运行原本被调用的函数。最后使用 setTimeout 以及配合个匿名函数实现延迟执行。

作为个简单的说明，比如会在通常的脚本中，编写下面的提供个字符串并弹出个警告框的简单函数：

``````````
function comms(s) {
  alert(s);
}
``````````

好了，后来我改变了我的想法。我想编写个「特殊版本」的函数，它会重复三次运行每次间隔两秒。那么使用我的 repeat 函数，就可以像这样做到：

``````````
var somethingWrong = repeat(comms, 3, 2000);
somethingWrong("Can you hear me, major tom?");
``````````

结果就犹如预期的那样，弹出了三次警告框每次延时两秒。

最后，arguments 即便不会经常被用到，甚至显得有些诡异，但是它上述的那些惊艳的功能（不仅仅是这些！）值得你去了解它。

\------------------------------以上转自：http://www.gracecode.com/archives/2551/--------------------------

看不太懂，转来好好琢磨。


[Andrew Tetlaw]: http://www.sitepoint.com/articlelist/487