---
title: stream.js
date: 2014-01-09 22:09:10
categories: "开发"
tags:
	- stream
	- js

---

stream.js 是一个很小、完全独立的Javascript类库，它为你提供了一个新的Javascript数据结构：streams.

``````````
<script src='stream-min.js'></script>
``````````


## streams是什么？ ##

Streams 是一个操作简单的数据结构，很像数组或链接表，但附加了一些**非凡的**能力。

## 它们有什么特别之处？ ##

跟数组不一样，streams是一个有魔法的数据结构。它可以装载**无穷多的元素**。是的，你没听错。他的这种魔力来自于具有*延后(lazily)执行*的能力。这简单的术语完全能表明它们可以加载无穷多的元素。

## 入门 ##

如果你愿意花10分钟的时间来阅读这篇文章，你对编程的认识有可能会被完全的改变(除非你有函数式编程的经验！)。请稍有耐心，让我来先介绍一下streams支持的跟数组或链接表很类似的基本功能操作。然后我会像你介绍一些它具有的非常有趣的特性。

Stream 是一种容器。它能容纳元素。你可以使用 `Stream.make` 来让一个stream加载一些元素。只需要把想要的元素当成参数传进去：

1.  **var** s = Stream.make( 10, 20, 30 ); // s is now a stream containing 10, 20, and 30

足够简单吧，现在 s 是一个拥有3个元素的stream： 10, 20, and 30; 有顺序的。我们可以使用 `s.length()` 来查看这个stream的长度，用 `s.item( i )` 通过索引取出里面的某个元素。你还可以通过调用 `s.head()` 来获得这个stream 的第一个元素。让我们实际操作一下：

1.  **var** s = Stream.make( 10, 20, 30 ); 
2.  console.log( s.length() ); // outputs 3
3.  console.log( s.head() ); // outputs 10
4.  console.log( s.item( 0 ) ); // exactly equivalent to the line above
5.  console.log( s.item( 1 ) ); // outputs 20
6.  console.log( s.item( 2 ) ); // outputs 30

本页面已经加载了这个 stream.js 类库。如果你想运行这些例子或自己写几句，打开你的浏览器的Javascript控制台直接运行就行了。

我们继续，我们也可以使用 `new Stream()` 或 直接使用 `Stream.make()` 来构造一个空的stream。你可以使用 `s.tail()` 方法来获取stream里除了头个元素外的余下所有元素。如果你在一个空stream上调用 `s.head()` 或 `s.tail()` 方法，会抛出一个异常。你可以使用 `s.empty()` 来检查一个stream是否为空，它返回 `true` 或 `false`。

1.  **var** s = Stream.make( 10, 20, 30 ); 
2.  **var** t = s.tail(); // returns the stream that contains two items: 20 and 30
3.  console.log( t.head() ); // outputs 20
4.  **var** u = t.tail(); // returns the stream that contains one item: 30
5.  console.log( u.head() ); // outputs 30
6.  **var** v = u.tail(); // returns the empty stream
7.  console.log( v.empty() ); // prints true

这样做可以打印出一个stream里的所有元素：

1.  **var** s = Stream.make( 10, 20, 30 ); 
2.  **while** ( !s.empty() ) \{ 
3.   console.log( s.head() ); 
4.   s = s.tail(); 
5.  \} 

我们有个简单的方法来实现这个： `s.print()` 将会打印出stream里的所有元素。

## 用它们还能做什么？ ##

另一个简便的功能是 `Stream.range( min, max )` 函数。它会返回一个包含有从 `min` 到 `max` 的自然数的stream。

1.  **var** s = Stream.range( 10, 20 ); 
2.  s.print(); // prints the numbers from 10 to 20

在这个stream上，你可以使用 `map`, `filter`, 和 `walk` 等功能。 `s.map( f )` 接受一个参数 `f`，它是一个函数， stream里的所有元素都将会被`f`处理一遍；它的返回值是经过这个函数处理过的stream。所以，举个例子，你可以用它来完成让你的 stream 里的数字翻倍的功能：

1.  **function** doubleNumber( x ) \{ 
2.  **return** 2 \* x; 
3.  \} 
4.  
5.  **var** numbers = Stream.range( 10, 15 ); 
6.  numbers.print(); // prints 10, 11, 12, 13, 14, 15
7.  **var** doubles = numbers.map( doubleNumber ); 
8.  doubles.print(); // prints 20, 22, 24, 26, 28, 30

很酷，不是吗？相似的， `s.filter( f )` 也接受一个参数`f`，是一个函数，stream里的所有元素都将经过这个函数处理；它的返回值也是个stream，但只包含能让`f`函数返回`true`的元素。所以，你可以用它来过滤到你的stream里某些特定的元素。让我们来用这个方法在之前的stream基础上构建一个只包含奇数的新stream：

1.  **function** checkIfOdd( x ) \{ 
2.  **if** ( x % 2 == 0 ) \{ 
3.  // even number
4.  **return****false**; 
5.   \} 
6.  **else** \{ 
7.  // odd number
8.  **return****true**; 
9.   \} 
10. \} 
11. **var** numbers = Stream.range( 10, 15 ); 
12. numbers.print(); // prints 10, 11, 12, 13, 14, 15
13. **var** onlyOdds = numbers.filter( checkIfOdd ); 
14. onlyOdds.print(); // prints 11, 13, 15

很有效，不是吗？最后的一个`s.walk( f )`方法，也是接受一个参数`f`，是一个函数，stream里的所有元素都要经过这个函数处理，但它并不会对这个stream做任何的影响。我们打印stream里所有元素的想法有了新的实现方法：

1.  **function** printItem( x ) \{ 
2.   console.log( 'The element is: ' \+ x ); 
3.  \} 
4.  **var** numbers = Stream.range( 10, 12 ); 
5.  // prints:
6.  // The element is: 10
7.  // The element is: 11
8.  // The element is: 12
9.  numbers.walk( printItem ); 

还有一个很有用的函数： `s.take( n )`，它返回的stream只包含原始stream里第前n个元素。当用来截取stream时，这很有用：

1.  **var** numbers = Stream.range( 10, 100 ); // numbers 10...100
2.  **var** fewerNumbers = numbers.take( 10 ); // numbers 10...19
3.  fewerNumbers.print(); 

另外一些有用的东西：`s.scale( factor )` 会用factor(因子)乘以stream里的所有元素； `s.add( t )` 会让 stream `s` 每个元素和stream `t`里对应的元素相加，返回的是相加后的结果。让我们来看几个例子：

1.  **var** numbers = Stream.range( 1, 3 ); 
2.  **var** multiplesOfTen = numbers.scale( 10 ); 
3.  multiplesOfTen.print(); // prints 10, 20, 30
4.  numbers.add( multiplesOfTen ).print(); // prints 11, 22, 33

尽管我们目前看到的都是对数字进行操作，但stream里可以装载任何的东西：字符串，布尔值，函数，对象；甚至其它的数组或stream。然而，请注意一定，stream里不能装载一些特殊的值：`null` 和 `undefined`。

## 想我展示你的魔力！ ##

现在，让我们来处理无穷多。你不需要往stream添加无穷多的元素。例如，在 `Stream.range( low, high )` 这个方法中，你可以忽略掉它的第二个参数，写成  `Stream.range( low )` ， 这种情况下，数据没有了上限，于是这个stream里就装载了所有从 low 到无穷大的自然数。你也可以把 `low` 参数也忽略掉，这个参数的缺省值是 `1` 。这种情况中， `Stream.range()` 返回的是所有的自然数。

## 这需要用上你无穷多的内存/时间/处理能力吗？ ##

不，不会。这是最精彩的部分。你可以运行这些代码，它们跑的非常快，就像一个普通的数组。下面是一个打印从 1 到 10 的例子：

1.  **var** naturalNumbers = Stream.range(); // returns the stream containing all natural numbers from 1 and up
2.  **var** oneToTen = naturalNumbers.take( 10 ); // returns the stream containing the numbers 1...10
3.  oneToTen.print(); 

## 你在骗人 ##

是的，我在骗人。关键是你可以把这些结构*想成*无穷大，这就引入了一种新的编程范式，一种致力于简洁的代码，让你的代码比通常的命令式编程更容易理解、更贴近自然数学的编程范式。这个Javascript类库本身就很短小；它是按照这种编程范式设计出来的。让我们来多用一用它；我们构造两个stream，分别装载所有的奇数和所有的偶数。

1.  **var** naturalNumbers = Stream.range(); // naturalNumbers is now 1, 2, 3, ...
2.  **var** evenNumbers = naturalNumbers.map( **function** ( x ) \{ 
3.  **return** 2 \* x; 
4.  \} ); // evenNumbers is now 2, 4, 6, ...
5.  **var** oddNumbers = naturalNumbers.filter( **function** ( x ) \{ 
6.  **return** x % 2 != 0; 
7.  \} ); // oddNumbers is now 1, 3, 5, ...
8.  evenNumbers.take( 3 ).print(); // prints 2, 4, 6
9.  oddNumbers.take( 3 ).print(); // prints 1, 3, 5

很酷，不是吗？我没说大话，stream比数组的功能更强大。现在，请容忍我几分钟，让我来多介绍一点关于stream的事情。你可以使用 `new Stream()` 来创建一个空的stream，用 `new Stream( head, functionReturningTail )` 来创建一个非空的stream。对于这个非空的stream，你传入的第一个参数成为这个stream的头元素，而第二个参数是一个*函数*，它返回stream的尾部(一个包含有余下所有元素的stream)，很可能是一个空的stream。困惑吗？让我们来看一个例子：

1.  **var** s = **new** Stream( 10, **function** () \{ 
2.  **return****new** Stream(); 
3.  \} ); 
4.  // the head of the s stream is 10; the tail of the s stream is the empty stream
5.  s.print(); // prints 10
6.  **var** t = **new** Stream( 10, **function** () \{ 
7.  **return****new** Stream( 20, **function** () \{ 
8.  **return****new** Stream( 30, **function** () \{ 
9.  **return****new** Stream(); 
10.  \} ); 
11.  \} ); 
12. \} ); 
13. // the head of the t stream is 10; its tail has a head which is 20 and a tail which
14. // has a head which is 30 and a tail which is the empty stream.
15. t.print(); // prints 10, 20, 30

没事找事吗？直接用`Stream.make( 10, 20, 30 )`就可以做这个。但是，请注意，这种方式我们可以轻松的构建我们的无穷大stream。让我们来做一个能够无穷无尽的stream：

1.  **function** ones() \{ 
2.  **return****new** Stream( 
3.  // the first element of the stream of ones is 1...
4.   1, 
5.  // and the rest of the elements of this stream are given by calling the function ones() (this same function!)
6.   ones 
7.   ); 
8.  \} 
9.  
10. **var** s = ones(); // now s contains 1, 1, 1, 1, ...
11. s.take( 3 ).print(); // prints 1, 1, 1

请注意，如果你在一个无限大的stream上使用 `s.print()`，它会无休无止的打印下去，最终耗尽你的内存。所以，你最好在使用`s.print()`前先`s.take( n )`。在一个无穷大的stream上使用`s.length()`也是无意义的，所有，不要做这些操作；它会导致一个无尽的循环(试图到达一个无尽的stream的尽头)。但是对于无穷大stream，你可以使用`s.map( f )` 和 `s.filter( f )`。然而，`s.walk( f )`对于无穷大stream也是不好用。所有，有些事情你要记住; 对于无穷大的stream，一定要使用`s.take( n )`取出有限的部分。

让我们看看能不能做一些更有趣的事情。还有一个有趣的能创建包含自然数的stream方式：

1.  **function** ones() \{ 
2.  **return****new** Stream( 1, ones ); 
3.  \} 
4.  **function** naturalNumbers() \{ 
5.  **return****new** Stream( 
6.  // the natural numbers are the stream whose first element is 1...
7.   1, 
8.  **function** () \{ 
9.  // and the rest are the natural numbers all incremented by one
10. // which is obtained by adding the stream of natural numbers...
11. // 1, 2, 3, 4, 5, ...
12. // to the infinite stream of ones...
13. // 1, 1, 1, 1, 1, ...
14. // yielding...
15. // 2, 3, 4, 5, 6, ...
16. // which indeed are the REST of the natural numbers after one
17. **return** ones().add( naturalNumbers() ); 
18.  \} 
19.  ); 
20. \} 
21. naturalNumbers().take( 5 ).print(); // prints 1, 2, 3, 4, 5

细心的读者会发现为什么新构造的stream的第二参数是一个返回尾部的函数、而不是尾部本身的原因了。这种方式可以通过延迟尾部截取的操作来防止进行进入无穷尽的执行周期。

让我们来看一个更复杂的例子。下面的是给读者留下的一个练习，请指出下面这段代码是做什么的？

1.  **function** sieve( s ) \{ 
2.  **var** h = s.head(); 
3.  **return****new** Stream( h, **function** () \{ 
4.  **return** sieve( s.tail().filter( **function**( x ) \{ 
5.  **return** x % h != 0; 
6.   \} ) ); 
7.   \} ); 
8.  \} 
9.  sieve( Stream.range( 2 ) ).take( 10 ).print(); 

请一定要花些时间能清楚这段代码的用途。除非有函数式编程经验，大多数的程序员都会发现这段代码很难理解，所以，如果你不能立刻看出来，不要觉得沮丧。给你一点提示：找出被打印的stream的头元素是什么。然后找出第二个元素是什么(余下的元素的头元素)；然后第三个元素，然后第四个。这个函数的名称也能给你一些提示。如果你对这种难题感兴趣，[这儿还有一些][Link 1]。

如果你*真的*想不出这段代码是做什么的，你就运行一下它，自己看一看！这样你就很容易理解它是怎么做的了。

## 致敬 ##

Streams 实际上不是一个新的想法。很多的函数式的编程语言都支持这种特征。所谓‘stream’是Scheme语言里的叫法，Scheme是LISP语言的一种方言。Haskell语言也支持无限大列表(list)。这些'take', 'tail', 'head', 'map' 和 'filter' 名字都来自于Haskell语言。Python和其它很多中语言中也存在虽然不同但很相似的这种概念，它们都被称作"发生器(generators)"。

这些思想来函数式编程社区里已经流传了很久了。然而，对于大多数的Javascript程序员来说却是一个很新的概念，特别是那些没有函数式编程经验的人。

这里很多的例子和创意都是来自 [Structure and Interpretation of Computer Programs][] 这本书。如果你喜欢这些想法，我高度推荐你读一读它；这本书可以在网上免费获得。它也是我开发这个Javascript类库的创意来源。

如果你喜欢其它语法形式的stream，你可以试一下[linq.js][]，或者，如果你使用 node.js， [node-lazy][] 也许更适合你。

如果你要是喜欢 CoffeeScript 的话， Michael Blume 正在把 stream.js 移植到 CoffeeScript 上，创造出 [coffeestream][]。

## 感谢你的阅读！ ##

我希望你能有所收获，并喜欢上 stream.js。这个类库是免费的，所以，如果你喜欢它，或它能在某方面提供了帮助，你可以考虑[替我买一杯热巧克力饮料][Link 2] (我不喝咖啡) 或者 [写信给我][Link 3]。如果你打算这样做，请写清你是哪里人，做什么的。我很喜欢收集世界各地的图片，所以，信中请附上你在你的城市里拍的照片！


\------------------------------------\------------------------------------\------------------------------------

\----------很抱歉 具体的出处忘记在哪里了. 我在整理电脑资源文件.想删除它.不过它确实不错.就转移到CSDN了。以下是其开源源码.

``````````
function Stream(head, tailPromise) {
	if (typeof head != 'undefined') {
		this.headValue = head;
	}
	if (typeof tailPromise == 'undefined') {
		tailPromise = function() {
			return new Stream();
		};
	}
	this.tailPromise = tailPromise;
}
Stream.prototype = {
	empty : function() {
		return typeof this.headValue == 'undefined';
	},
	head : function() {
		if (this.empty()) {
			throw 'Cannot get the head of the empty stream.';
		}
		return this.headValue;
	},
	tail : function() {
		if (this.empty()) {
			throw 'Cannot get the tail of the empty stream.';
		}
		return this.tailPromise();
	},
	item : function(n) {
		if (this.empty()) {
			throw 'Cannot use item() on an empty stream.';
		}
		var s = this;
		while (n != 0) {
			--n;
			try {
				s = s.tail();
			} catch (e) {
				throw 'Item index does not exist in stream.';
			}
		}
		try {
			return s.head();
		} catch (e) {
			throw 'Item index does not exist in stream.';
		}
	},
	length : function() {
		var s = this;
		var len = 0;
		while (!s.empty()) {
			++len;
			s = s.tail();
		}
		return len;
	},
	add : function(s) {
		return this.zip(function(x, y) {
			return x + y;
		}, s);
	},
	zip : function(f, s) {
		if (this.empty()) {
			return s;
		}
		if (s.empty()) {
			return this;
		}
		var self = this;
		return new Stream(f(s.head(), this.head()), function() {
			return self.tail().zip(f, s.tail());
		});
	},
	map : function(f) {
		if (this.empty()) {
			return this;
		}
		var self = this;
		return new Stream(f(this.head()), function() {
			return self.tail().map(f);
		});
	},
	reduce : function(aggregator, initial) {
		if (this.empty()) {
			return initial;
		}
		return this.tail().reduce(aggregator, aggregator(initial, this.head()));
	},
	sum : function() {
		return this.reduce(function(a, b) {
			return a + b;
		}, 0);
	},
	walk : function(f) {
		this.map(function(x) {
			f(x);
			return x;
		}).force();
	},
	force : function() {
		var stream = this;
		while (!stream.empty()) {
			stream = stream.tail();
		}
	},
	scale : function(factor) {
		return this.map(function(x) {
			return factor * x;
		});
	},
	filter : function(f) {
		if (this.empty()) {
			return this;
		}
		var h = this.head();
		var t = this.tail();
		if (f(h)) {
			return new Stream(h, function() {
				return t.filter(f);
			});
		}
		return t.filter(f);
	},
	take : function(howmany) {
		if (this.empty()) {
			return this;
		}
		if (howmany == 0) {
			return new Stream();
		}
		var self = this;
		return new Stream(this.head(), function() {
			return self.tail().take(howmany - 1);
		});
	},
	drop : function(n) {
		var self = this;
		while (n-- > 0) {
			if (self.empty()) {
				return new Stream();
			}
			self = self.tail();
		}
		return new Stream(self.headValue, self.tailPromise);
	},
	member : function(x) {
		var self = this;
		while (!self.empty()) {
			if (self.head() == x) {
				return true;
			}
			self = self.tail();
		}
		return false;
	},
	print : function(n) {
		var target;
		if (typeof n != 'undefined') {
			target = this.take(n);
		} else {
			target = this;
		}
		target.walk(function(x) {
			console.log(x);
		});
	},
	toString : function() {
		return '[stream head: ' + this.head() + '; tail: ' + this.tail() + ']';
	}
};
Stream.makeOnes = function() {
	return new Stream(1, Stream.makeOnes);
};
Stream.makeNaturalNumbers = function() {
	return new Stream(1, function() {
		return Stream.makeNaturalNumbers().add(Stream.makeOnes());
	});
};
Stream.make = function() {
	if (arguments.length == 0) {
		return new Stream();
	}
	var restArguments = Array.prototype.slice.call(arguments, 1);
	return new Stream(arguments[0], function() {
		return Stream.make.apply(null, restArguments);
	});
};
Stream.range = function(low, high) {
	if (typeof low == 'undefined') {
		low = 1;
	}
	if (low == high) {
		return Stream.make(low);
	}
	return new Stream(low, function() {
		return Stream.range(low + 1, high);
	});
};
``````````




[Link 1]: http://streamjs.org/sequence-challenge.js
[Structure and Interpretation of Computer Programs]: http://mitpress.mit.edu/sicp/full-text/book/book.html
[linq.js]: http://neue.cc/reference.htm
[node-lazy]: https://github.com/pkrumins/node-lazy
[coffeestream]: https://github.com/MichaelBlume/coffeestream
[Link 2]: http://www.aqee.net/docs/stream/#donate-form
[Link 3]: mailto:dionyziz@gmail.com