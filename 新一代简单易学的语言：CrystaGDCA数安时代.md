---
title: 新一代简单易学的语言：Crysta
date: 2018-06-20 14:47:23
categories: "开发"
tags:
	- 编译器
	- Rust
	- LLVM
	- 编程语言
	- Ruby

---

Crystal是一个非常年轻的语言，是一门基于 LLVM 的可编译的语言，它整个设计尽可能的接近 Ruby 的体验，语法、各种标准库的接口都几乎和 Ruby 一样（兼容不是它的目标）,但又有 C 一样的性能（官方给出的某些测试数据表明）。

特性：

语法类似 Ruby Ruby-inspired syntax.

无需指定变量类型和方法参数类型 Never have to specify the type of a variable or method argument.

可以调用 C 代码 Be able to call C code by writing bindings to it in Crystal.

编译时代码模拟和生成 Have compile-time evaluation and generation of code, to avoid boilerplate code.

编译成高效的本机代码 Compile to efficient native code.

![新一代简单易学的语言：Crysta][Crysta]

示例（使用埃拉托色尼筛选法计算100以内的���数和的Crystal代码如下）：

Crystal代码

max = 100

sieve = Array.new(max, true)

sieve\[0\] = false

(2...max).each do |i|

if sieve\[i\]

(2 \* i).step(max - 1, i) do |j|

sieve\[j\] = false

end

end

end

sieve.each\_with\_index do |prime, number|

puts number if prime

end

Crystal的Hello World：

Crystal代码

puts "Hello World"

你也可以使用面向对象方法：

Crystal代码

class Greeter

def initialize(name)

@name = name.capitalize

end

def salute

puts "Hello \#\{@name\}!"

end

end

g = Greeter.new("world")

g.salute

或者使用块：

Crystal代码

"Hello World".each\_char do |char|

print char

end

print ' '

Ruby-ists使用Crystal的五大理由

1.极低的学习曲线

过去流行的语言有Elixir，Go，Rust，它们都比Ruby有性能优势，但更难以学习和掌握。

而Crystal却是更容易的学习曲线，获得性能增益。

多么容易？我们来看看一些代码。

问：下列哪些模块是用Ruby编写的？哪些在Crystal？

module Year

def self.leap?(year)

year % 400 == 0 || (year % 100 != 0 && year % 4 == 0)

end

end

module Hamming

def self.distance(a,b)

a.chars.zip(b.chars).count\{|first, second| first != second \}

end

end

上述模块将以Ruby或Crystal工作。

但这并不意味着所有的Ruby代码都可以在Crystal中使用（反之亦然），但是可以通过Crystal做很多事情，高效工作。

Crystal（强类型和编译语言）如何像Ruby（一种动态和鸭型语��）一样？Crystal的编译器使用两种强大技术的组合：类型推断和联合类型。这允许编译器读取类似ruby的代码并找出（推断）要使用的正确类型。

除了相似点之外，Crystal提供了比Ruby更多的核心优势。

2.编译时间检查和方法重载

Crystal是一种编译语言，在编译时检查所有的方法输入和输出。如果任何类型不匹配，它们将在运行前被捕获。

上面的例子。在Ruby中，当输入不是整数时会发生什么？

Year.leap?("2016") \#=> false

Year.leap?(Date.new(2016, 1, 1)) \#=> undefined method \`%' for \#<Date: 2016-01-01 ... >

对于String得到的错误答案，Date得到一个运行时异常。修复Ruby中的东西至少需要一条语句：

module Year

def self.leap?(input)

if input.is\_a? Integer

input % 400 == 0 || (input % 100 != 0 && input % 4 == 0)

elsif input.is\_a? Date

input.leap?

else

raise ArgumentError.new("must pass an Integer or Date.")

end

end

end

在Crystal中，我们可以选择显式输入输入（和输出）。我们可以改变方法签名self.leap?(year : Int)，我们保证有一个整数作为输入。

我们在编译时得到有用的消息，而不是运行时：

Year.leap?("2016")

Error in line 10: no overload matches 'Year.leap?' with type String

Overloads are:

\- Year.leap?(year : Int)

如果在模块中添加对Ruby的支持Time（DateTime我们可以考虑使用Ruby），那么可以重载Year::leap?：

module Year

def self.leap?(year : Int)

year % 400 == 0 || (year % 100 != 0 && year % 4 == 0)

end

def self.leap?(time : Time)

self.leap?(time.year)

end

end

像Ruby一样，方法重载允许输入的灵活性，但没有鸭子输入的猜测。编译时间检查防止类型不匹配错误导致生产失败。

3.快速的表现

编译的另一个优点是速度和优化。通常，比较Ruby和Crystal的性能时，可以用数量级而不是百分比来表示。

在一个例子中，将Crystal���的随机数加起来可以比Ruby 快10个数量级（约快37％）。这是由于编译器优化以及在Crystal中使用原始数据类型的能力。这确实伴随着大数字整数溢出的风险（参见Ary的解释）。

基准测试中， Crystal内置的HTTP服务器每秒能够处理超过200万个请求。许多Web框架一直为Web应用程序提供亚毫秒级的响应时间。

4.想要的网页框架已经在这里

你想要一个完整的框架，利用编译时间检查强参数，HTTP动词和数据库查询吗？

1月份，这些Web框架中的每一个都将在他们自己的专门帖子中突出显示。查看Crystal的博客，了解更多信息。

5.Crystal是用Crystal书写的！很容易理解并对语言做出贡献

Ruby的实现 Enumerable\#all?

static VALUE

enum\_all(int argc, VALUE \*argv, VALUE obj)

\{

struct MEMO \*memo = MEMO\_ENUM\_NEW(Qtrue);

rb\_block\_call(obj, id\_each, 0, 0, ENUMFUNC(all), (VALUE)memo);

return memo->v1;

\}

需要���长时间才能弄清楚代码在做什么？如果从未使用过C代码，那可能是很长时间的。

与Crystal的实现相比Enumerable\#all?

def all?

each \{ |e| return false unless yield e \}

true

end

需要多长时间才能弄清楚？如果知道Ruby或Crystal，可能只需几秒钟。

考虑到这一点，考虑到98.4％的Crystal是用Crystal编写的，只有0.3％是用C ++编写的。

Ruby在C语言中占30.6％，在Ruby中占64.8％。

事实上，Crystal中只有一个文件是用C ++编写的，所以只要不改变LLVM扩展，无论在Crystal语言中寻找什么，都可以保证用Crystal写入。

————————————————————————————

SSL证书是HTTP明文协议升级HTTPS加密协议的重要渠道，是网络安全传输的加密通道。关于更多SSL证书的资讯，请关注数安时代（GDCA）。GDCA致力于网络信息安全，已通过WebTrust 的国际认证，是全球可信任的证���签发机构。GDCA专业技术团队将根据用户具体情况为其提供最优的产品选择建议，并针对不同的应用或服务器要求提供专业对应的HTTPS解决方案。


[Crysta]: /pro/os/crawler/VFQB-NNYZ-N7RB.jpg
 *  **原文作者：** GDCA数安时代
 *  **原文链接：** https://www.toutiao.com/item/6569054738749325828/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。