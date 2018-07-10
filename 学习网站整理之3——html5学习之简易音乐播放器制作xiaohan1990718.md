---
title: 学习网站整理之3——html5学习之简易音乐播放器制作
date: 2013-10-14 20:42:02
categories: "开发"
tags:
	- 学习网站
	- html

---

为了学点html的东西 ，简单将这个所谓额简单播放器嵌套在我目前整理的网站里，作为随机“主页”的一部分-如上次上次的资料里所写，现在是按照星期的顺序，当打开时，每天都有相应的主页网站，当然之前都是百度，本来想用google的，无奈出现的那个问题我暂时没经历去研究了。

![FFUZ-EZJ2-YIQJ.jpg][]


如上图所示：图中显示的有简单的html5里的audio标签元素 另外我加了3个按钮 分别表示上一曲 重新听 和下一曲。

还添加了一个div 用于显示目前播放的曲目名称。

在网页上添加了键盘监听事件 监听了左右键，用于切换歌曲。

JS代码大致如下：

``````````
<!DOCTYPE HTML>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
    <title>我的歌曲</title>
</head>
<body>

<!--<video src="1.mp3" controls="controls" id="myVideo">
</video >-->
<audio src="1.mp3" controls="controls" id="myVideo"></audio>
<div id="musicName"></div>
<button onclick="play(-1)">上一曲</button><button onclick="play(0)">重新听</button><button onclick="play(1)">下一曲</button>
<script>
    var musicArr = ["(韩德尔) - 最缓板.mp3", "- 绝对真实的BABY笑声(搞怪另类铃声首选).mp3", "30552812.mp3", "30575109.mp3", "7 Days (Album Version).mp3", "Aint Sayin Nothin.mp3", "Are You The One.mp3", "Ballet.mp3", "Beat It.mp3", "Bela Bartok - 第三号钢琴协奏曲（彼得杜诺荷）.mp3", "Billie Jean.mp3", "Bon Jovi - Save the World.mp3", "Celine Dion - My Heart Will Go On (Titanic).mp3", "Dangerous.mp3", "Daughters.mp3", "Daughtry - Home.mp3", "DEPAPEPE - Flow.mp3", "Fall Again.mp3", "Fiona Joy Hawkins - 第二乐章“永恒之爱”.mp3", "Hey Jude.mp3", "Home (Album Version).mp3", "I Finally Found Someone.mp3", "I Just Can'T Stop Loving You.mp3", "I Like It, I Love It.mp3", "I Told You So.mp3", "I'll Be Fine.mp3", "Ice Box.mp3", "Jar Of Love.mp3", "LALALOVE.mp3", "Lovefool.mp3", "Matthew West - Family Tree.mp3", "Mobile.mp3", "No Woman No Cry.mp3", "Pacific Moon - A So Bi Ma Sho.mp3", "Red Bean 红豆.mp3", "Reflection.mp3", "Resident Evil Suite.mp3", "Resta In Ascolto.mp3", "Rush Of Fools - Grace Found Me.mp3", "Satisfied.mp3", "Should It Matter.mp3", "Soul Boy.mp3", "Stellar Kart - Everything Is Different Now.mp3", "Take Me To Your Heart(With Hyesung).mp3", "The Final Countdown.mp3", "The Rasmus - Sail Away.mp3", "Theory of a Deadman - Easy To Love You.mp3", "Traveling Light.mp3", "Trouble Is A Friend.mp3", "Under The Mango Tree.mp3", "Valsa Carioca.mp3", "Vindicated.mp3", "We On feat Yo Gotti.mp3", "WestLife - i lay my love on you.mp3", "WestLife - my love.mp3", "WestLife - You Raise Me Up.mp3", "Yalli Nassi.mp3", "Yesterday.mp3", "yiyong.mp3", "Ʈѩ.mp3", "一个人.mp3", "一生何求.mp3", "一生所爱.mp3", "一生有你.mp3", "下雨的时候会想你.mp3", "不要用我的爱来伤害我.mp3", "不要说话.mp3", "世间情歌.mp3", "中国好声音.mp3", "他一定很爱你.mp3", "你的样子.mp3", "信乐团 - 海阔天空.mp3", "偏偏喜欢你.mp3", "冬季到台北来看雨.mp3", "冷雨夜.mp3", "刀郎 - 永远的兄弟.mp3", "努力活着.mp3", "勃拉姆斯 - 摇篮曲.mp3", "千百度.mp3", "原来你也在这里.mp3", "可否冲破.mp3", "司文 - 光棍好苦.mp3", "周华健 - 难念的经.mp3", "回心转意.mp3", "在水一方.mp3", "外婆的澎湖湾.mp3", "夜生带走最后一个我.mp3", "大海.mp3", "天堂.mp3", "天天看到你.mp3", "天黑.mp3", "女儿情.mp3", "她来听我的演唱会.mp3", "如果这都不算爱.mp3", "姑娘我爱你.mp3", "娃娃 - 漂洋过海来看你.mp3", "孙燕姿 - 开始懂了.mp3", "左右为难.mp3", "差一点.mp3", "巴达捷夫斯卡 - 少女的祈祷.mp3", "庐州月.mp3", "张学友 - 每次都想呼喊你的名字.mp3", "张宇 - 用心良苦.mp3", "张宇 - 男人的好.mp3", "张悬 - 宝贝.mp3", "张惠妹 - 我可以抱你吗.mp3", "张栋梁 - 当你孤单你会想起谁.mp3", "弯弯的月亮.mp3", "归来吧.mp3", "德沃夏克 - 寂静的森林（选自g小调大提琴与乐队）.mp3", "德沃夏克 - 幽默曲.mp3", "心的方向.mp3", "思念是一种病.mp3", "恋曲1990.mp3", "情书.mp3", "想和你去吹吹风.mp3", "想把我唱给你听.mp3", "我一定要得到你.mp3", "我们爱这个错.mp3", "我们都是好孩子.mp3", "我是真的真的很爱妳.mp3", "我的好兄弟.mp3", "我的歌声里.mp3", "掀起你的盖头来.mp3", "断桥残雪.mp3", "断点(国).mp3", "明天会更好.mp3", "曼托瓦尼 - 晨曲.mp3", "有多少爱可以重来.mp3", "有没有人告诉你.mp3", "李民浩 - 城市猎人.mp3", "李贞贤 - 说吧.mp3", "柴可夫斯基 - 如歌的行板.mp3", "每一步.mp3", "汪峰 - 我们的爱情.mp3", "沙宝亮 - 暗香.mp3", "没有你的日子我真的好孤单.mp3", "没有情人的情人节.mp3", "浪子心声.mp3", "海鸣威 - 老人与海.mp3", "涛声依旧.mp3", "清明雨上.mp3", "温哥华悲伤一号.mp3", "漂洋过海来看你.mp3", "灌篮高手 - 只凝视着你（灌篮高手插曲）.mp3", "灌篮高手 - 好想大声叫喜欢你（灌篮高手片头曲）.mp3", "灌篮高手 - 直到世界尽头.mp3", "灰色头像.mp3", "爱你在心口难开.mp3", "爱的代价.mp3", "牛朝阳 - 爱情乞丐.mp3", "牧羊曲.mp3", "玻璃杯.mp3", "电台情歌.mp3", "痒.mp3", "痴心的我.mp3", "白狐.mp3", "相爱的泪水.mp3", "相见不如怀念.mp3", "离别.mp3", "秋天不回来.mp3", "空城.mp3", "突然的自我.mp3", "笑红尘.mp3", "筷子兄弟 - 祝福你亲爱的.mp3", "筷子兄弟 - 老男孩.mp3", "红色石头.mp3", "纯音乐 - 卡农.mp3", "纯音乐 - 圣母颂 (舒伯特).mp3", "美人吟.mp3", "羞答答的玫瑰静悄悄的开.mp3", "群星 - C大调第3弦奏鸣曲 中板.mp3", "群星 - 小抒情调 (巴赫).mp3", "老师你好.mp3", "肖邦 - 雨滴前奏曲.mp3", "胎教音乐 - 小溪和月亮的歌儿（C大调长笛与竖琴协奏曲）.mp3", "胎教音乐 - 布兰登堡协奏曲.mp3", "舒伯特 - 小夜曲（选自《天鹅之歌》）.mp3", "舒曼 - 梦幻曲（选自《童年情景》）.mp3", "英格兰民歌 - 你的秋波使我陶醉.mp3", "萨顶顶 - 万物生 (中文版).mp3", "蓝莲花.mp3", "西城男孩 - my love.mp3", "西海情歌.mp3", "触电.mp3", "说好的幸福呢.mp3", "谢军 - 那一夜.mp3", "谷村新司.-.风姿花传.mp3", "贝多芬 - 月光奏鸣曲.mp3", "贝多芬 - 给爱丽丝.mp3", "输了你赢了世界有如何.mp3", "迈克尔·杰克逊 - You Are Not Alone (Radio Edit).mp3", "追梦人.mp3", "那些花儿.mp3", "酸酸甜甜就是我.mp3", "铁齿铜牙纪晓岚.mp3", "镜子.面具.mp3", "门德尔松 - 乘着歌声的翅膀（钢琴版）.mp3", "陈奕迅_王菲 - 因为爱情.mp3", "陈小春 - 我爱的人.mp3", "陈楚生 - 倾国倾城.mp3", "雨蝶.mp3", "青春舞曲.mp3", "韩宝仪 - 粉红色的回忆.mp3", "风往北吹.mp3", "马友友 - 德弗札克：B小调大提琴协奏曲，作品104 第三乐章：终乐章.mp3", "鲁冰花.mp3", "黄小琥 - 白天不懂夜的黑（清晰版）.mp3", "黄昏.mp3"],
        defaultIndex=0;//默认、当前指向的索引
    var myVideo = document.getElementById("myVideo");
    var musicName = document.getElementById("musicName");
    //musicName.innerHTML="现在播放的是:"+ ;
    function play(i){
       defaultIndex = defaultIndex + i;
        if(defaultIndex<=0){
            defaultIndex = 0;
        }else if(defaultIndex>=musicArr.length-1){
            defaultIndex = musicArr.length-1;
        }
        myVideo.src = "file:///E:/Music/"+musicArr[defaultIndex];
        myVideo.autoplay="autoplay";

        musicName.innerHTML="现在播放的是:"+ musicArr[defaultIndex];
    }

   document.onkeyup = function(e){
       var key = e.keyCode;
       if(key==37){
           play(-1);
       }
       if(key==39){
           play(1);
       }


   }

</script>
</body>
</html>
``````````


当然，你看到的上述歌曲用数组维护的，我不会一个一个的写上去的，遍历歌曲文件的Java代码如下：

``````````
public static void main(String[] args) {
		File folder = new File("E:\\Music");
		File[] files = folder.listFiles();
		List<String> list = new ArrayList<String>();
		for(File f : files){
			if(f.getName().endsWith(".mp3")){
				list.add("\""+f.getName()+"\"");
			}
		}
		System.out.println(list);

	}
``````````


然后将控制待输出的信息 copy到这个数组里来就行了。

暂是想那么多，今后页面美化和连续播放的问题会继续更新～～




[FFUZ-EZJ2-YIQJ.jpg]: /pro/os/crawler/FFUZ-EZJ2-YIQJ.jpg