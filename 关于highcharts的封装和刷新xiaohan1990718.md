---
title: 关于highcharts的封装和刷新
date: 2014-02-11 21:05:57
categories: "开发"
tags:
	- chart

---

前言:

很气愤,在更改不知道被改过几手的代码.惨不忍睹.之前做这块的一个小家伙年前跑路了～留下这一烂摊子,重复代码量多的很想有种全部删除重写的冲动.可考虑我还有其他任务在身,只好作罢.在原先基础上尽可能的修复。

\--------------- \---------------------------------------------------------------------------------------------------------

项目中用到了highcharts.一个好好的图形组件,让之前经手人写的乱七八糟,客户也很变态,一说 需要在切换 图例的时候动态的显示/隐藏y轴.于是就有了之前的那篇文章。不过现在回想起来,客户的真实需求变成代码的话 应该是动态的切换y轴. 话说我分析需求的能力还是蛮对的. 又一说 想把图形里的字体全部变动 刚好切中要害. 因为我刚好有动力将highcharts本身重新组装一遍,方便今后的需求变化时,我一个个找这些图形 copy 粘贴！！！

先说总结:

这里如标题 只说封装(简单封装和隔离封装两种) 以及图形的刷新。

1、原先所谓的刷新是创建另一个highcharts对象,将其渲染到上一个highcharts对象渲染到的id上.或者直接对highcharts里的属性通过赋值的方式进行修改(还好,不过重复代码太多.).

2、highcharts跟jQuery结合 可以使用jQuery对象提供的一些方法进行update\\setData 操作.达到刷新的效果.

3、这里重点提到隔离封装里的刷新方法,一遍今后逐步的更新扩展,尽量支持更多情景。

``````````
var HQ = HQ||{};
			HQ._copy = function(p,c){
				for(var key in c){
					p[key] = c[key];
				}
			};
			HQ.MyHighChart = function(cfg){
				/*************************基础配置声明区和默认配置区域*************************/	
				var chart = {
				},credits = {
					enabled : false
				},title ={
					text:''
				},xAxis ={
					labels:{
						formatter:function(){
							return '<span style="color:#ff0000">'+this.value+'</span>';
						}
					},
					//gridLineWidth : 1,//网格(竖线)宽度
					//lineWidth : 1,//基线(x轴 轴线)宽度
                    categories: []
				},yAxis =[]
				,tooltip = {
                      crosshairs: true
                },series =[];
				// yAxis  series 需要特殊的处理(暂时先不做控制) 其余均按照copy的方式更改  
                
				/*********************************参数处理区域*********************************/	
				HQ._copy(chart,cfg.chart||{});
				HQ._copy(credits,cfg.credits||{});
				HQ._copy(title,cfg.title||{});
				HQ._copy(xAxis,cfg.xAxis||{});
				HQ._copy(tooltip,cfg.tooltip||{});
				//yAxis(数组或对象) : lineWidth : 1  不要做一些控制 
					 yAxis = cfg.yAxis||[];
					 series = cfg.series||[];
				
				//删除上述被使用过的对象.是为了兼容除目前其他程序员在使用highchart其他的配置属性
				delete cfg.chart;	
				delete cfg.credits;
				delete cfg.title;
				delete cfg.xAxis;
				delete cfg.tooltip;
				delete cfg.yAxis;
				delete cfg.series;
				var baseHighChartCfg = {
						chart:chart,
						credits:credits,
						title:title,
						xAxis:xAxis,
						yAxis:yAxis,
						tooltip:tooltip,
						series:series
				};
				HQ._copy(baseHighChartCfg,cfg)
				//创建highchart
				var highChart =  new Highcharts.Chart(baseHighChartCfg);
				return highChart;
			}
``````````


通过上述代码对highcharts进行的简单的包装,这样的好处有:

1、今后可以根据实际需求对默认值进行特殊话修改

2、以credits = \{enabled : false\}为例,highcharts原生对象生成后会顺带highcharts图标.这样为了隐藏或者变更它,重复代码量过大.

在这个基础上将 return highChart 删除,取而代之的是:

``````````
/*********************************公共接口(highchart常用的提供处来)区域*********************************/
				this.series =highChart.series;
				this.setTitle = function(){
					highChart.setTitle.apply(highChart,arguments);
				};
				this.addSeries =  function(){
					highChart.addSeries.apply(highChart,arguments);
				};
				this.redraw = function(){
					 highChart.redraw.apply(highChart,arguments);
				};
				this.showLoading = function(){
					highChart.showLoading.apply(highChart,arguments);
				};
				this.hideLoading = function(){
					highChart.hideLoading.apply(highChart,arguments);
				};
				//刷新方法处理(暂:支持的刷新包括标题和series)
				this.refrash = function(cfg){
					highChart.setTitle(cfg.title||{});
					var seriersArr = cfg.series||[],series= highChart.series; 
					if(seriersArr.length){//先清除所有x轴 再添加
						while(series.length > 0) {
							series[0].remove(false);
						}
						for(var i=0,len=seriersArr.length;i<len;i++){
							highChart.addSeries(seriersArr[i],false);
						}
						highChart.redraw();
					}
				};
``````````


这样以来,每当我们创建一个包装后的highcharts对象后,我们就无法直接操控原生highcharts对象,就达到了某种控制的目的.如:假设今后为了适应项目发展.我们更换highcharts，用其他的图形组件,那这时候我们就不需要去一个个修改之前的代码了～.

当然这样会限制一个特殊属性或方法,导致一些情况下难以实现.但相较今后的需求变化来说.这么做是值得的。

重点说一下refrash方法. 现在支持对title和x轴的刷新.里面还缺乏很多情况的处理.导致我对y轴目前没很好的想法去控制它动态的创建或销毁.（显示和隐藏 见前篇）

附上效果:

![V6FF-JIRJ-UFM3.jpg][]![J2QA-EEYQ-YUE2.jpg][]



上述两图.图一为初次生成的效果 图2是动态刷新后的效果.

原文资源已上传至我的资源。


[V6FF-JIRJ-UFM3.jpg]: /pro/os/crawler/V6FF-JIRJ-UFM3.jpg
[J2QA-EEYQ-YUE2.jpg]: /pro/os/crawler/J2QA-EEYQ-YUE2.jpg