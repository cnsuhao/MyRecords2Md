---
title: 一个关于农历的算法----js实现【转】
date: 2013-07-13 19:51:25
categories: "开发"
tags:
	- js

---

<!-- 中国农历开始 -->

<SCRIPT language=JavaScript>

<!--

var lunarInfo=new Array(

0x04bd8,0x04ae0,0x0a570,0x054d5,0x0d260,0x0d950,0x16554,0x056a0,0x09ad0,0x055d2,

0x04ae0,0x0a5b6,0x0a4d0,0x0d250,0x1d255,0x0b540,0x0d6a0,0x0ada2,0x095b0,0x14977,

0x04970,0x0a4b0,0x0b4b5,0x06a50,0x06d40,0x1ab54,0x02b60,0x09570,0x052f2,0x04970,

0x06566,0x0d4a0,0x0ea50,0x06e95,0x05ad0,0x02b60,0x186e3,0x092e0,0x1c8d7,0x0c950,

0x0d4a0,0x1d8a6,0x0b550,0x056a0,0x1a5b4,0x025d0,0x092d0,0x0d2b2,0x0a950,0x0b557,

0x06ca0,0x0b550,0x15355,0x04da0,0x0a5d0,0x14573,0x052d0,0x0a9a8,0x0e950,0x06aa0,

0x0aea6,0x0ab50,0x04b60,0x0aae4,0x0a570,0x05260,0x0f263,0x0d950,0x05b57,0x056a0,

0x096d0,0x04dd5,0x04ad0,0x0a4d0,0x0d4d4,0x0d250,0x0d558,0x0b540,0x0b5a0,0x195a6,

0x095b0,0x049b0,0x0a974,0x0a4b0,0x0b27a,0x06a50,0x06d40,0x0af46,0x0ab60,0x09570,

0x04af5,0x04970,0x064b0,0x074a3,0x0ea50,0x06b58,0x055c0,0x0ab60,0x096d5,0x092e0,

0x0c960,0x0d954,0x0d4a0,0x0da50,0x07552,0x056a0,0x0abb7,0x025d0,0x092d0,0x0cab5,

0x0a950,0x0b4a0,0x0baa4,0x0ad50,0x055d9,0x04ba0,0x0a5b0,0x15176,0x052b0,0x0a930,

0x07954,0x06aa0,0x0ad50,0x05b52,0x04b60,0x0a6e6,0x0a4e0,0x0d260,0x0ea65,0x0d530,

0x05aa0,0x076a3,0x096d0,0x04bd7,0x04ad0,0x0a4d0,0x1d0b6,0x0d250,0x0d520,0x0dd45,

0x0b5a0,0x056d0,0x055b2,0x049b0,0x0a577,0x0a4b0,0x0aa50,0x1b255,0x06d20,0x0ada0)

var Animals=new Array("鼠","牛","虎","兔","龙","蛇","马","羊","猴","鸡","狗","猪");

var Gan=new Array("甲","乙","丙","丁","戊","己","庚","辛","壬","癸");

var Zhi=new Array("子","丑","寅","卯","辰","巳","午","未","申","酉","戌","亥");

var now = new Date();

var SY = now.getYear();

var SM = now.getMonth();

var SD = now.getDate();


function cyclical(num) \{ return(Gan\[num%10\]+Zhi\[num%12\]) \} //==== 传入 offset 传回干支, 0=甲子

//==== 传回农历 y年的总天数

function lYearDays(y) \{

var i, sum = 348

for(i=0x8000; i>0x8; i>>=1) sum += (lunarInfo\[y-1900\] & i)? 1: 0

return(sum+leapDays(y))

\}


//==== 传回农历 y年闰月的天数

function leapDays(y) \{

if(leapMonth(y)) return((lunarInfo\[y-1900\] & 0x10000)? 30: 29)

else return(0)

\}


//==== 传回农历 y年闰哪个月 1-12 , 没闰传回 0

function leapMonth(y) \{ return(lunarInfo\[y-1900\] & 0xf)\}


//====================================== 传回农历 y年m月的总天数

function monthDays(y,m) \{ return( (lunarInfo\[y-1900\] & (0x10000>>m))? 30: 29 )\}


//==== 算出农历, 传入日期物件, 传回农历日期物件

// 该物件属性有 .year .month .day .isLeap .yearCyl .dayCyl .monCyl

function Lunar(objDate) \{

var i, leap=0, temp=0

var baseDate = new Date(1900,0,31)

var offset = (objDate - baseDate)/86400000


this.dayCyl = offset + 40

this.monCyl = 14


for(i=1900; i<2050 && offset>0; i++) \{

temp = lYearDays(i)

offset -= temp

this.monCyl += 12

\}

if(offset<0) \{

offset += temp;

i--;

this.monCyl -= 12

\}


this.year = i

this.yearCyl = i-1864


leap = leapMonth(i) //闰哪个月

this.isLeap = false


for(i=1; i<13 && offset>0; i++) \{

//闰月

if(leap>0 && i==(leap+1) && this.isLeap==false)

\{ --i; this.isLeap = true; temp = leapDays(this.year); \}

else

\{ temp = monthDays(this.year, i); \}


//解除闰月

if(this.isLeap==true && i==(leap+1)) this.isLeap = false


offset -= temp

if(this.isLeap == false) this.monCyl ++

\}


if(offset==0 && leap>0 && i==leap+1)

if(this.isLeap)

\{ this.isLeap = false; \}

else

\{ this.isLeap = true; --i; --this.monCyl;\}


if(offset<0)\{ offset += temp; --i; --this.monCyl; \}


this.month = i

this.day = offset + 1

\}


function YYMMDD()\{

var cl = '<font color="green" STYLE="font-size:13pt;">';

if (now.getDay() == 0) cl = '<font color="\#c00000" STYLE="font-size:13pt;">';

if (now.getDay() == 6) cl = '<font color="green" STYLE="font-size:13pt;">';

return(cl+SY+'年'+(SM+1)+'月'+'</font>');

\}

function weekday()\{

var day = new Array("星期日","星期一","星期二","星期三","星期四","星期五","星期六");

var cl = '<font color="green" STYLE="font-size:9pt;">';

if (now.getDay() == 0) cl = '<font color="green" STYLE="font-size:9pt;">';

if (now.getDay() == 6) cl = '<font color="red" STYLE="font-size:9pt;">';

return(cl+ day\[now.getDay()\]+ '</font>');

\}

//==== 中文日期

function cDay(m,d)\{

var nStr1 = new Array('日','一','二','三','四','五','六','七','八','九','十');

var nStr2 = new Array('初','十','廿','卅','　');

var s;

if (m>10)\{s = '十'+nStr1\[m-10\]\} else \{s = nStr1\[m\]\} s += '月'

switch (d) \{

case 10:s += '初十'; break;

case 20:s += '二十'; break;

case 30:s += '三十'; break;

default:s += nStr2\[Math.floor(d/10)\]; s += nStr1\[d%10\];

\}

return(s);

\}

function solarDay1()\{

var sDObj = new Date(SY,SM,SD);

var lDObj = new Lunar(sDObj);

var cl = '<font color="\#9933CC" STYLE="font-size:9pt;">';

var tt = '【'+Animals\[(SY-4)%12\]+'】'+cyclical(lDObj.monCyl)+'月 '+cyclical(lDObj.dayCyl++)+'日' ;

return(cl+tt+'</font>');

\}

function solarDay2()\{

var sDObj = new Date(SY,SM,SD);

var lDObj = new Lunar(sDObj);

var cl = '<font color="green" STYLE="font-size:9pt;">';

//农历BB'+(cld\[d\].isLeap?'闰 ':' ')+cld\[d\].lMonth+' 月 '+cld\[d\].lDay+' 日

var tt = cyclical(SY-1900+36)+'年 '+cDay(lDObj.month,lDObj.day);

return(cl+tt+'</font>');

\}

function solarDay3()\{

var sTermInfo = new Array(0,21208,42467,63836,85337,107014,128867,150921,173149,195551,218072,240693,263343,285989,308563,331033,353350,375494,397447,419210,440795,462224,483532,504758)

var solarTerm = new Array("小寒","大寒","立春","雨水","惊蛰","春分","清明","谷雨","立夏","小满","芒种","夏至","小暑","大暑","立秋","处暑","白露","秋分","寒露","霜降","立冬","小雪","大雪","冬至")

var lFtv = new Array("0101\*春节","0115 元宵节","0505 端午节","0707 七夕情人节","0715 中元节","0815 中秋节","0909 重阳节","1208 腊八节","1224 小年","0100\*除夕")

var sFtv = new Array("0101\*元旦","0214 情人节","0308 妇女节","0309 偶今天又长一岁拉","0312 植树节","0315 消费者权益日","0401 愚人节","0418 MM的生日","0501 劳动节","0504 青年节","0512 护士节","0601 儿童节","0701 建党节 香港回归纪念","0801 建军节","0808 父亲节","0909 毛席逝世纪念","0910 教师节","0928 孔子诞辰","1001\*国庆节",

"1006 老人节","1024 联合国日","1112 孙中山诞辰","1220 澳门回归纪念","1225 圣诞节","1226 毛席诞辰")


var sDObj = new Date(SY,SM,SD);

var lDObj = new Lunar(sDObj);

var lDPOS = new Array(3)

var festival='',solarTerms='',solarFestival='',lunarFestival='',tmp1,tmp2;

//农历节日

for(i in lFtv)

if(lFtv\[i\].match(/^(\\d\{2\})(.\{2\})(\[\\s\\\*\])(.+)$/)) \{

tmp1=Number(RegExp.$1)-lDObj.month

tmp2=Number(RegExp.$2)-lDObj.day

if(tmp1==0 && tmp2==0) lunarFestival=RegExp.$4

\}

//国历节日

for(i in sFtv)

if(sFtv\[i\].match(/^(\\d\{2\})(\\d\{2\})(\[\\s\\\*\])(.+)$/))\{

tmp1=Number(RegExp.$1)-(SM+1)

tmp2=Number(RegExp.$2)-SD

if(tmp1==0 && tmp2==0) solarFestival = RegExp.$4

\}

//节气

tmp1 = new Date((31556925974.7\*(SY-1900)+sTermInfo\[SM\*2+1\]\*60000)+Date.UTC(1900,0,6,2,5))

tmp2 = tmp1.getUTCDate()

if (tmp2==SD) solarTerms = solarTerm\[SM\*2+1\]

tmp1 = new Date((31556925974.7\*(SY-1900)+sTermInfo\[SM\*2\]\*60000)+Date.UTC(1900,0,6,2,5))

tmp2= tmp1.getUTCDate()

if (tmp2==SD) solarTerms = solarTerm\[SM\*2\]


if(solarTerms == '' && solarFestival == '' && lunarFestival == '')

festival = '';

else

festival = '<TABLE WIDTH=100% BORDER=0 CELLPADDING=2 CELLSPACING=0 BGCOLOR="\#CCFFCC"><TR><TD align=center><marquee direction=left scrolldelay=120 behavior=alternate>'+

'<FONT COLOR="\#FF33FF" STYLE="font-size:9pt;"><b>'+solarTerms + ' ' + solarFestival + ' ' + lunarFestival+'</b></FONT></marquee></TD>'+

'</TR></TABLE>';

var cl = '<font color="green" STYLE="font-size:9pt;">';

return(cl+festival+'</font>');

\}


//显示当前时间

function CurentTime()

\{

var now = new Date();

var hh = now.getHours();

var mm = now.getMinutes();

var ss = now.getTime() % 60000;

ss = (ss - (ss % 1000)) / 1000;

var clock = hh+':';

if (mm < 10) clock += '0';

clock += mm+':';

if (ss < 10) clock += '0';

clock += ss;

return(clock);

\}


function refreshCalendarClock() //

\{

document.all.ClockTime.innerHTML = CurentTime();

\}

//显示当前时间


function setCalendar()\{

document.write("<table border='1' cellspacing='3' width='180' bordercolor='\#009B00' bgcolor='\#FFFFFF' height='110' cellpadding='2'");

document.write("<tr><td align='center'><b>"+YYMMDD()+"<br><font face='Arial' size='6' color=\#FF8040>"+SD+"</font><br>");

document.write(weekday()+"<br><font id=ClockTime color=red></font>"+"<br></b>");

document.write(solarDay1()+"<br>"+solarDay2()+"<br>"+solarDay3()+"</td></tr></table>");


\}

setCalendar();

setInterval('refreshCalendarClock()',1000);//1秒钟刷新1次当前时间

//-->

</SCRIPT>

<!--结束-->


===关于农历===


农 历

　　农历是我国的一种历法，又称夏历、中历、旧历，俗称阴历。定月的方法是用朔望月周期给出，朔所在日为初一，朔望月长约29天半，所以农历大月30天，小月29天。农历平年有十二个月，全年354天或355天，闰年为十三个月，其中某一月为闰月，月名依前一月名而定，如前月是八月，闰月则为闰八月。闰年全年383天或384天。


设置闰月的方法是：

农历月份中无“中气”的月份则是闰月。

农历平年、闰年的月数、天数一览表:


年　　月数　大月天数　小月天数　全年天数　　　　闰月设置方法

平年　　12　　30　　　　29　　　　354　　　　　　大约19年中7个闰月

闰年　　13　　30　　　　29　　　　383（或384）　 无中气月份为闰月


二十四节气中四季“节气”和“中气”一览表:


四季 春 夏 秋 冬 节气 立春 惊蛰 清明 立夏 芒种 小暑 立秋 白露 寒露 立冬 大雪 小寒 中气 雨水 春分 谷雨 小满 夏至 大暑 处暑 秋分 霜降 小雪 冬至 大寒


　　农历又根据太阳的位置，把太阳年分成二十四个节气，反映寒冷暑热的气候变化，以便家事活动，所以农历实为阴阳历。

如何转换阴阳历?

　　很多人都一直在找换阴阳历的公式。我也尝试过。曾读过「高平子」天文前辈所着「学历散论」了解古历的变更和阴阳历的缺陷。才知道由於月球转动的不稳定不规则，确定无公式可寻。这也是古代中国每百年必改历的原因。

阴历最大的问题是在如何置闰。好像不难，因为阴历基本法则如下:

　\* 月朔日即是初一

　\* 月以中气得名

　\* 以包含雨水中气月为正月，即是「寅」月

　\* 月无中气者为闰月，以前月同名

　　如果，日月转动循还有规则的话， 推演一套阴阳历转换的公式并不难。问题在有时一个太阴月比一个太阳月还要长。如此一个太阴月就有可能包括两个中气。此双中气月後的阴历月名就全部乱掉了，直到下一个「假」闰月後才调整过来。

　　一般人接触到的阴阳历是民用历法，它是政府颁令的以东经120度计算的历法或称中原标准时间或北京时。如果，我们用不同时区、不同经度为子午线来重新计算阴阳历，民用历法的置闰法则出了很大的问题。不同时区的闰月可能落在不同月。换言之，在一百年内，任何两个时区的闰月顺序模式是会不相同的。


　　高平子前辈书中提到了「历理置闰法」。如果应用历理置闰法到不同时区，则所有不同时区的闰月都落在相同月。如此不同时区、不同经度的阴阳历置闰之问题就消失了。民用置闰和历理置闰的不同是：

　　\* 在民用置闰，如果月朔日和中气同一天，则该阴历月包含那个中气。

　　\* 在历理置闰，如果月朔日和中气同一天，月朔日时间必须在中气时间之前，则该阴历月才包含那个中气。

　　简言之，民用置闰比较月朔和中气日期；历理置闰比较月朔和中气日期、时、分、秒。由此可知，没有精确的太阳和月亮的时间数字，阴历的闰月可能会排错了。

　　基於这些理由，我着手寻找天文公式计算精确的太阳和月亮在纬度的时间。当年没有网路，发了大半年於美国南加州各大图书馆及大学，找寻答案。1993年出版了「中美天文万年历」一书。书中精确的天文日月时间只从1900到2010年。因恐2011後时间误差超过一分钟，不够精确，不敢印出。今年2002从网路资讯，确定太阳和月亮时间的精确度後，百忙中重新整理资料，提供给需要阴阳历转换公式的朋友。

　　整理出的太阳和月亮时间数字是从西元1年到2246年。有历理和中国民用两套历法。数字内容清清楚楚的看出民用历法的敝端。例如，从西元1600年到2246年，民用历法双中气的阴历月有22个，历理历法只有5个。民用历法甚至在2033、2128和2242年中，三个月之间居然跑出两个双中气；换言之，三个月中多出两个「假」闰月。前後12个阴历月中有三个闰月，闰月的去留造成许多学者的讨论和困恼。历理历法在此三年中，却没有发现到双中气阴历月。闰月的去留只要把双中气月後的「假」闰月取消，则历理历法近乎於完美。

　　由此可知，民用历法问题很大，应该废除。上次阴阳历重大改历在1645年，已经超过350年。随着天文科学的进步，中国阴阳历应该使用较精确的历理历法。免得後代子孙再浪费时间讨论置闰去留的问题。

　　阴阳历应用在八字算命、紫微斗数、农民历、遁甲历最多。很多人不知道排八字只用阳历而不用阴历。发了许多时间在研究阴阳历的转换。其实八字只使用太阳中节气，和月亮没有任何关系。紫微斗数则需要阴历日期去排命盘。美国时区的阴历日期有一半和中国时区的阴历日期差一天，因为时差超过12小时；初一就可能在不同日之故。有位在加拿大职业算命的朋友，精通八字和紫微斗数，研究其女命盘。八字论父母，合情合理。斗数父母宫，看不出自己影子。後来，在中美天文万年历一书发现了「差一天」之解答。

　　曾有一位退休博士用了近三十年找寻阴阳历的公式，问遍两岸各大天文台，得不到答案，直到发现中美天文万年历一书。最後，希望我重新整理的中国阴阳历的天文数字和原始程式，能给有求知欲於阴阳历转换方法的读友一个答案，以免得不到答案而遗憾终身。


中国阴阳历的天文数字和原始程式用简单英文阐述，

请从 Chinese Lunar Calendar 进入。

form:http://www.cnblogs.com/xian4432/archive/2009/12/27/1633490.html

有空会做一个功能齐全的日期组件～暂保留与此予以借鉴。