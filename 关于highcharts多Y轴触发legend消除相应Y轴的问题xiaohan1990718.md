---
title: 关于highcharts多Y轴触发legend消除相应Y轴的问题
date: 2014-01-26 19:55:25
categories: "开发"
tags:
	- chart

---

因为某些缘故,从fusioncharts的热爱转移到highcharts的研究,不过时间的问题并没事深入学习它。

本文主要是为了解决一个问题,我不知道highcharts官方有没有注意到一个问题,它提供了多Y轴共享的情形,并且提供了触发legend时对应的线隐藏并且相应的y轴的刻度也会消失(隐藏).但当我的需求再进一步的时候:我需要Y轴的文字说明以及轴也隐藏或消失的时候,很多highcharts用户傻眼了.因为官方没提供啊,并且它的接口也少的可怜,兼容性问题也导致很多时候一些接口不能用。

以下是我的解决思路:

因为之前对fusioncharts的研究有过一段血泪史,基本上连它源码都翻了一遍,后来对图形也慢慢有了认知.当接到这个需求的时候,第一反应是我要得到用户触发legend的事件监听,并在监听里做对Y轴控制的处理。

果不其然,很顺利可以获取到legend的监听,翻译过来叫做‘图例’,图例的监听有两个,一个是颜色,另一个是文字.当默认的情况下我们触发它其中的一个,就会导致相应的线显示或隐藏。

``````````
$(chart.series).each(function(i,c){//c
	c = c||chart.series[i];
	if(c.legendSymbol&&c.legendSymbol.on){
		c.legendSymbol.on('click',function(e){ 
			
		});
	}
	if(c.legendItem&&c.legendItem.on){
		c.legendItem.on('click',function(e){
			
		});
	}
});
``````````


以上chart.seriers是highcharts的seriers对应的数组对象,我姑且懒了一次使用jquery的each方法对它进行遍历,并且得到数组中每一个对象。

得到对象后利用调试工具(firebug\\chrome自带的等等)均可查看该对象的属性和方法.其中便发现了legendItem、legendSymbol两个属性（对象）,并继续查看得到戏那个应的on方法.一般来说框架中的on \\ addlisteners方法均是添加事件监听的。因此拿来尝试,便得到了对其单击事件的控制方法～

第二步:

得到监听后还需要得到监听函数的回调函数传递给的参数.或者直接传入该对象,对每一y轴进行处理.

``````````
var clickFn = function(i,yAxisOne){
	if(!yAxisOne.axisTitle.__oldText){
		yAxisOne.axisTitle.__oldText = yAxisOne.axisTitle.text;
	}
	if(yAxisOne.axisTitle.element.lastChild.innerHTML){
		yAxisOne.axisTitle.element.lastChild.innerHTML = '';
	}else{
		yAxisOne.axisTitle.element.lastChild.innerHTML = yAxisOne.axisTitle.__oldText;
	}
}
$(chart.series).each(function(i,c){//c
	c = c||chart.series[i];
	if(c.legendSymbol&&c.legendSymbol.on){
		c.legendSymbol.on('click',function(e){ 
			clickFn(i,c.yAxis);
		});
	}
	if(c.legendItem&&c.legendItem.on){
		c.legendItem.on('click',function(e){
			clickFn(i,c.yAxis);
		});
	}
});
``````````


以上clickFn函数是用于处理相应的监听.方法内部即是对Y轴的控制.因为写的仓促,没有进一步优化,上述的 i 参数 以及相应的

``````````
c.legendSymbol&&c.legendSymbol.on
``````````

等判断,是可以省略的,当时是为了配合测试数据有误的情况下做的兼容。

\--------------------------------------------------------------------------------------------------------------------------------

这样的话就解决了y轴文字的隐藏和显示问题.

但遗留了最大的一个问题是,我现在还找不到相应的属性和方法来控制 y轴 本轴的显示/隐藏问题. 为了屏蔽这个问题,我想了一个比较‘聪明’的办法.






。

/

。

/





那就是不显示相应的 y 轴那条 ‘纵’线...... 突然感觉这个办法好NB的样子.当然偶也顺利的找到关于把highcharts 的y轴‘纵’轴的线隐藏的方式:

enabled属性设置为 false 即它完全不会显示y轴


这个属性我没尝试,不知道是不是,但是我看demo确定的是官方提供了某个属性是可以将'纵'轴隐藏的。

\--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


http://jsfiddle.net/gh/get/jquery/1.9.1/highslide-software/highcharts.com/tree/master/samples/highcharts/demo/line-basic/
