---
title: WebAssembly会是Web 的将来吗
date: 2018-02-10 16:40:25
categories: "开发"
tags:
	- 编译器
	- 汇编语言
	- JavaScript
	- 编程语言
	- Safari

---

WebAssembly 的目标是 Web 的字节码。未来的开发者会这样使用 WebAssembly：开发一个应用(源码用任何可以编译到 WebAssembly 的语言)用编译器把源码转换到 WebAssembly 字节码在浏览器加载字节码并执行。
![WebAssembly会是Web 的将来吗][WebAssembly_Web]使用WebAssembly，我们可以在浏览器中运行一些高性能、低级别的编程语言，可用它将大型的C和C++代码库比如游戏、物理引擎甚至是桌面应用程序导入Web平台。截至目前为止，我们已经可以在Chrome、Firefox中使用WebAssembly，Edge和Safari对它的支持也基本完成。这意味着很快，就能在所有流行的浏览器中运行wasm了。

# WebAssembly 不是 JavaScript 的末日 #

JavaScript 依然会在 Web 平台上继续增长, JavaScript 已经成为开发者和工具链的通用语言，WebAssembly 不会改变这一点。WebAssembly 想要替代的是 JavaScript用作编译目标的底层的代码表示形式，随着越来越多语言和平台以 Web 为目标, JavaScript 有着更多的压力，浏览器提供商为此尽可能加入缺失的功能，其中一些功能在已经很复杂的 JavaScript 语义当中不能运作地很好。WebAssembly 就是为这个问题提供解决方案:它从一开始就被设计为编译目标，它将得到所有主流浏览器提供商的支持，它可以根据需要，任意地偏离 JavaScript 原有的语义，WebAssembly 对于 Web 来说是对 JavaScript 很有必要的补充。

# WebAssembly 不是汇编  #

相对于其他底层代码表示形式,或者大多数的二进制代码, WebAssembly 描述的是抽象语法树(AST), WebAssembly 提供了高级的结构, 比如循环和分支，意味着可以直接写 WebAssembly, 或者反编译二进制文件，从而得到可读性比较好的 opcodes 或者指令，WebAssembly 将支持编译后文件增加调试信息，方便调试。

# WebAssembly 会扩展出 JavaScript 所没有的功能 #

WebAssembly 初步的实现目标是等同 ASM.js 的功能.也就是说, 能用 ASM.js 做的, 等 WebAssembly 推出之后你也能做(做得更好)，第一个版本的可以期待的一个改进是更好的加载时间，WebAssembly 的二进制版本比起 ASM.js 的文本格式可以更快地解析，所以即便是最初的版本, WebAssembly 也能显示出改进。当前版本的 WebAssembly 叫做最小可行产品，将来的版本可以期待有更多的改进。

# Source Maps 帮助你在浏览器中轻松调试编译后的代码 #

编译目标语言的一个不足是调试常常变得更难，如果你使用过现在编译到 JavaScript 的任何语言,你大概体会到过调试有多困难, 要靠记忆力把编译结果映射到源码，WebAssembly 的目标是成为其他语言优秀平台, 所以针对方案已经在做了，跟现今的本地编译器很像, WebAssembly 将允许二进制中存在调试信息,以及 Source Maps, 用来告诉浏览器和调试工具怎么样映射生成代码到源码，易于调试是作为 WebAssembly 规范的一部分的

# 题外话 #

WebAssembly作为一种新兴的Web技术，相关的资料和社区还不够丰富，但其为web开发提供了一种崭新的思路和工作方式，未来是很有可能大放光彩的。



[WebAssembly_Web]: /pro/os/crawler/FIQN-UVER-MMBE.jpg
 *  **原文作者：** XiaoQi程序猿
 *  **原文链接：** https://www.toutiao.com/item/6520204322280571395/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。