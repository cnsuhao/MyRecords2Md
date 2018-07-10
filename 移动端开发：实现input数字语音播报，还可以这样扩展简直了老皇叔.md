---
title: 移动端开发：实现input数字语音播报，还可以这样扩展简直了
date: 2017-06-28 21:49:39
categories: "开发"
tags:
	- 移动端

---

网页移动端H5开发中，我们时常遇到输入手机号码的场景，这里以手机号码（数字）语音播放为切入点，供大家学习。

![移动端开发：实现input数字语音播报，还可以这样扩展简直了][input]

你往下看的是Javascript技术教程


**实现思路：**

1.  语音对象，用�����语音播放，可实现按url插入顺序播放，实现未播放url删除不读。
2.  input数字控制对象，用于控制输入内容。

**支持：**

1.  中英文数字语音播放，默认使用百度翻译语音程序。
2.  支持自定义数字语音路径。
3.  方便控制混合app开发时，语音路径置换为app本地路径，减少网络请求，提高读取速度。
4.  原生和jQuery两种方式实现。

**代码部分：**


``` html

<html>

<head>

<titleNew Document </title>

<meta charset="utf-8">

<meta name="Generator" content="EditPlus">

<meta name="Author" content="">

<meta name="Keywords" content="">

<meta name="Description" content="">

<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">

<meta name=”viewport” content=”width=device-width; initial-scale=1.0; maximum-scale=1.0; user-scalable=0;” />

</head>

<script type="text/javascript" src="http://www.linuxidc.com/inc/jquery.js"></script>

<script src="http://api.html5media.info/1.1.6/html5media.min.js"></script>

<script src="http://vjs.zencdn.net/ie8/1.1.2/videojs-ie8.min.js"></script>

<script type="text/javascript">

<!--

var SpeechVideo = function(){

this.urls = [];

this.canload = true;

this.setInterval = null;

this.audio = null;

};

SpeechVideo.prototype.__load = function(){

var me = this;

if(this.canload == false){ return; }

var url = this.urls.shift();

if(url == null && this.setInterval != null){ clearInterval(this.setInterval); this.setInterval = null; return; }else{ this.play(url); }

if(this.setInterval == null){

this.setInterval = setInterval(function(){

me.__load();

},10);

}

}

SpeechVideo.prototype.play = function(url){

this.canload = false;

if(this.audio == null){

if(typeof Audio != 'undefined'){ this.audio = new Audio(); }else{

//兼容IE9-

if(typeof $ !== 'undefined'){

var $audio = $('<audio src="'+ url +'" controls preload></audio>').hide();

$audio.appendTo(document.body);

this.audio = $audio[0];

}else{

var audio = document.createElement('audio');

audio.style.display="none";

document.body.appendChild(audio);

this.audio = audio;

}

}

}

if(typeof Audio != 'undefined'){

var audio = this.audio,me = this;

audio.src = url;

if(window.attachEvent){

audio.onloadstart = function(){ me.canload = false;};

audio.onended = function() { me.canload = true; };

audio.onerror = function() { me.canload = true; };

}else{

audio.addEventListener('loadstart',function(){ me.canload = false;});

audio.addEventListener('ended',function() { audio.currentTime = 0; me.canload = true; });

audio.addEventListener('error',function() { audio.currentTime = 0; me.canload = true; });

}

audio.play();

}else{

//兼容IE9-【暂时没找到合适的方法，以网易云音乐为例的话，估计有眉目】

}

}

SpeechVideo.prototype.next = function(url){

this.urls.push(url);

this.__load();

};

SpeechVideo.prototype.remove = function(ele){

if(typeof ele == 'undefined'){ this.urls.pop(); }else

if(typeof ele == 'number'){

if(ele this.urls.length - 1){ ele = this.urls.length - 1; }

this.urls.splice(ele,1);

}else{

var index = -1;

for (var i = 0; i < this.urls.length; i++) {

if (this.urls[i] == ele) return index = i;

}

if(index -1){ this.urls.splice(index,1); }

}

}

var NumberSpeech = function(cfg){

var dfCfg = {

input : null, reg : null ,

srcsMap : { //英文发音

'1' : 'http://fanyi.baidu.com/gettts?lan=en&text=One&spd=3&source=web',

'2' : 'http://fanyi.baidu.com/gettts?lan=en&text=Two&spd=3&source=web',

'3' : 'http://fanyi.baidu.com/gettts?lan=en&text=Three&spd=3&source=web',

'4' : 'http://fanyi.baidu.com/gettts?lan=en&text=Four&spd=3&source=web',

'5' : 'http://fanyi.baidu.com/gettts?lan=en&text=Five&spd=3&source=web',

'6' : 'http://fanyi.baidu.com/gettts?lan=en&text=Six&spd=3&source=web',

'7' : 'http://fanyi.baidu.com/gettts?lan=en&text=Seven&spd=3&source=web',

'8' : 'http://fanyi.baidu.com/gettts?lan=en&text=Eight&spd=3&source=web',

'9' : 'http://fanyi.baidu.com/gettts?lan=en&text=Nine&spd=3&source=web',

'0' : 'http://fanyi.baidu.com/gettts?lan=en&text=Zero&spd=3&source=web'

}//输入容器【支持ID 和 jquery对象两种形式】 ， 输入规则 , 发音资源

};

var type = navigator.appName,lang = (type == "Netscape" ? navigator.language : navigator.userLanguage).substr(0, 2);

if (lang == "zh"){

dfCfg.srcsMap = { //中文发音

'1' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E4%B8%80&spd=5&source=web',

'2' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E4%BA%8C&spd=5&source=web',

'3' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E4%B8%89&spd=5&source=web',

'4' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E5%9B%9B&spd=5&source=web',

'5' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E4%BA%94&spd=5&source=web',

'6' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E5%85%AD&spd=5&source=web',

'7' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E4%B8%83&spd=5&source=web',

'8' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E5%85%AB&spd=5&source=web',

'9' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E4%B9%9D&spd=5&source=web',

'0' : 'http://fanyi.baidu.com/gettts?lan=zh&text=%E9%9B%B6&spd=5&source=web',

};

}

if(typeof $ !== 'undefined'){

dfCfg = $.extend(true, dfCfg, cfg || {});

}else{

//浅层次copy

var srcs = cfg.srcsMap || {};

delete cfg.srcsMap;

for(var key in (cfg || {})){ dfCfg[key] = cfg[key]; }

for(var key in srcs){

dfCfg.srcsMap[key] = srcs[key];

}

}

if(dfCfg.input == null){ throw '请指定当��容��'; }

var $input = dfCfg.input,speechVideo = new SpeechVideo(),oldLength = 0,isIE = typeof window.attachEvent !== 'undefined';

if(typeof $ !== 'undefined'){ //TODO !== 为测试原生方式暂设置 ==

if(typeof dfCfg.input == 'string'){ $input = $('\#'+dfCfg.input);}

$input.bind('keydown',function(event){

if(event.keyCode == 8 && isIE){

//var curValue = $(this).val();

//var len = oldLength - curValue.length;

//for(var i=0;i<len;i++){

speechVideo.remove(); oldLength--; if(oldLength < 0){ oldLength = 0;}

//}

}

});

$input.bind("input propertychange change",function(){

var curValue = $(this).val();

if(oldLength curValue.length){ speechVideo.remove(); }else{

var last = curValue.substring(curValue.length - 1);

if(dfCfg.srcsMap[last]){

speechVideo.next(dfCfg.srcsMap[last]);

}

}

oldLength = curValue.length;

});

}else{

var inputObj = document.getElementById(dfCfg.input),objEvent;

oldLength = inputObj.value.length;

inputObj.onkeypress = function(event){

var keyCode = event.keyCode;

event.returnValue = (keyCode >= 48 && keyCode <= 57);

};

inputObj.onkeydown = function(event){

if(event.keyCode == 8 && isIE){ speechVideo.remove(); oldLength--; if(oldLength < 0){ oldLength = 0;}}

}

objEvent = function(event){

var curValue = '';

if (typeof event.srcElement != 'undefined' && event.propertyName == "value") {

curValue = event.srcElement.value;

}else if(typeof event.target != 'undefined'){ curValue = event.target.value; }

if(oldLength curValue.length){ speechVideo.remove(); }else{

var last = curValue.substring(curValue.length - 1);

if(dfCfg.srcsMap[last]){

speechVideo.next(dfCfg.srcsMap[last]);

}

}

oldLength = curValue.length;

}

//input.onpropertychange = input.oninput = input.onchange

if(isIE){ inputObj.onpropertychange = objEvent; }else{ inputObj.oninput = objEvent; }

}

}

window.onload = function(){

new NumberSpeech({

input : 'numberspeech'

});

}

//-->

</script>

<body>

<input type="text" id="numberspeech"/>

</body>

</html>
>
```

**未尽事项：**

IE9- 的支持，使用audio.js 不过这里不深究，毕竟应用场景在APP端，跟微软IE关系没多大。这里demo已引用支持，可自行实现。若应用于PC端，多选移除时，内部oldValue计算方式不合理，这暂不在考虑范围内，可自行优化，难度不大～

**思路扩展：**

利用百度语音（以“http://fanyi.baidu.com/gettts?lan=zh&text=%E4%BA%94&spd=5&source=web”为例，text对应的值，即为语音���容）���实现语音播放时很轻松的。

附注： 该文原发自我个人技术博客，今后会逐步迁移到今日头条，以期形成系列教程，还原订阅关注，技术无止境，但愿一路有你。

![移动端开发：实现input数字语音播报，还可以这样扩展简直了][input 1]

你没看错，老皇叔是个程序猿



[input]: /pro/os/crawler/2INQ-MZ3Y-NVMF.jpg
[input 1]: /pro/os/crawler/FMZN-MZBY-YJQV.jpg
