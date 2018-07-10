---
title: html5学习之音乐播放器的结构整理
date: 2013-10-15 22:19:23
categories: "开发"
tags:
	- html

---

花了一点时间 整理下昨天的播放器，将结构优化了点，方便今后维护和扩展。

![VBEY-QAEA-7NMI.jpg][]


如昨天的想法一样，又添加了播放/暂停的切换按钮和随机切换。

另外的界面样式以及歌曲显示部分 暂时没空搞了。

思路如下：再添加一个div 用于展现数组里的歌曲，其中我还不清楚js是否可以动态加载我磁盘里的文件，感觉应该可以的，这里不谈，用数组代替。

展现的歌曲根据数组索引进行分页和展示，添加相应的单击事件 方便用户选择喜欢的歌曲 分页以方便展示 另外可以添加一个搜索框 用于查询相应关键字的歌曲。

也可以设置歌曲的播放模式、是否搜索歌曲的播放...等等。 今后有想法了 再扩展吧。 要睡了...

``````````
<!DOCTYPE HTML>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
    <title>我的MusicPlayer</title>
</head>
<script>
    MyUtil = {
        /**
         * 添加单击事件监听
         * @param htmlElement
         * @param callback
         * @param scope
         */
        addClickEvent:function(htmlElement,callback,scope){
            htmlElement.onclick = function(event){
                callback.call(scope||this,event);
            }
        },
        /**
         * 生产随机数
         * @param m
         * @param n
         * @return {Number}
         */
        randomInt:function(m,n){
            if(!n){
                n=m;
                m=0;
            }
            if(m>n){
                var temp = m;
                m=n;
                n = temp;
            }
            return Math.floor(Math.random() *(n-m+1)+m);
        }
    };
    MyMusicPlayer = function(){
        var obj = arguments[0]||{};//以对象形式传入，格式暂未定
       this.isInitPaly = obj['isInitPlay']==true;//是否初始化播放
        this.musicList =  ["(韩德尔) - 最缓板.mp3", "- 绝对真实的BABY笑声(搞怪另类铃声首选).mp3", "30552812.mp3", "30575109.mp3", "7 Days (Album Version).mp3", "Aint Sayin Nothin.mp3", "Are You The One.mp3", "Ballet.mp3", "Beat It.mp3", "Bela Bartok - 第三号钢琴协奏曲（彼得杜诺荷）.mp3", "Billie Jean.mp3", "Bon Jovi - Save the World.mp3", "Celine Dion - My Heart Will Go On (Titanic).mp3", "Dangerous.mp3", "Daughters.mp3", "Daughtry - Home.mp3", "DEPAPEPE - Flow.mp3", "Fall Again.mp3", "Fiona Joy Hawkins - 第二乐章“永恒之爱”.mp3", "Hey Jude.mp3", "Home (Album Version).mp3", "I Finally Found Someone.mp3", "I Just Can'T Stop Loving You.mp3", "I Like It, I Love It.mp3", "I Told You So.mp3", "I'll Be Fine.mp3", "Ice Box.mp3", "Jar Of Love.mp3", "LALALOVE.mp3", "Lovefool.mp3", "Matthew West - Family Tree.mp3", "Mobile.mp3", "No Woman No Cry.mp3", "Pacific Moon - A So Bi Ma Sho.mp3", "Red Bean 红豆.mp3", "Reflection.mp3", "Resident Evil Suite.mp3", "Resta In Ascolto.mp3", "Rush Of Fools - Grace Found Me.mp3", "Satisfied.mp3", "Should It Matter.mp3", "Soul Boy.mp3", "Stellar Kart - Everything Is Different Now.mp3", "Take Me To Your Heart(With Hyesung).mp3", "The Final Countdown.mp3", "The Rasmus - Sail Away.mp3", "Theory of a Deadman - Easy To Love You.mp3", "Traveling Light.mp3", "Trouble Is A Friend.mp3", "Under The Mango Tree.mp3", "Valsa Carioca.mp3", "Vindicated.mp3", "We On feat Yo Gotti.mp3", "WestLife - i lay my love on you.mp3", "WestLife - my love.mp3", "WestLife - You Raise Me Up.mp3", "Yalli Nassi.mp3", "Yesterday.mp3", "yiyong.mp3", "Ʈѩ.mp3", "一个人.mp3", "一生何求.mp3", "一生所爱.mp3", "一生有你.mp3", "下雨的时候会想你.mp3", "不要用我的爱来伤害我.mp3", "不要说话.mp3", "世间情歌.mp3", "中国好声音.mp3", "他一定很爱你.mp3", "你的样子.mp3", "信乐团 - 海阔天空.mp3", "偏偏喜欢你.mp3", "冬季到台北来看雨.mp3", "冷雨夜.mp3", "刀郎 - 永远的兄弟.mp3", "努力活着.mp3", "勃拉姆斯 - 摇篮曲.mp3", "千百度.mp3", "原来你也在这里.mp3", "可否冲破.mp3", "司文 - 光棍好苦.mp3", "周华健 - 难念的经.mp3", "回心转意.mp3", "在水一方.mp3", "外婆的澎湖湾.mp3", "夜生带走最后一个我.mp3", "大海.mp3", "天堂.mp3", "天天看到你.mp3", "天黑.mp3", "女儿情.mp3", "她来听我的演唱会.mp3", "如果这都不算爱.mp3", "姑娘我爱你.mp3", "娃娃 - 漂洋过海来看你.mp3", "孙燕姿 - 开始懂了.mp3", "左右为难.mp3", "差一点.mp3", "巴达捷夫斯卡 - 少女的祈祷.mp3", "庐州月.mp3", "张学友 - 每次都想呼喊你的名字.mp3", "张宇 - 用心良苦.mp3", "张宇 - 男人的好.mp3", "张悬 - 宝贝.mp3", "张惠妹 - 我可以抱你吗.mp3", "张栋梁 - 当你孤单你会想起谁.mp3", "弯弯的月亮.mp3", "归来吧.mp3", "德沃夏克 - 寂静的森林（选自g小调大提琴与乐队）.mp3", "德沃夏克 - 幽默曲.mp3", "心的方向.mp3", "思念是一种病.mp3", "恋曲1990.mp3", "情书.mp3", "想和你去吹吹风.mp3", "想把我唱给你听.mp3", "我一定要得到你.mp3", "我们爱这个错.mp3", "我们都是好孩子.mp3", "我是真的真的很爱妳.mp3", "我的好兄弟.mp3", "我的歌声里.mp3", "掀起你的盖头来.mp3", "断桥残雪.mp3", "断点(国).mp3", "明天会更好.mp3", "曼托瓦尼 - 晨曲.mp3", "有多少爱可以重来.mp3", "有没有人告诉你.mp3", "李民浩 - 城市猎人.mp3", "李贞贤 - 说吧.mp3", "柴可夫斯基 - 如歌的行板.mp3", "每一步.mp3", "汪峰 - 我们的爱情.mp3", "沙宝亮 - 暗香.mp3", "没有你的日子我真的好孤单.mp3", "没有情人的情人节.mp3", "浪子心声.mp3", "海鸣威 - 老人与海.mp3", "涛声依旧.mp3", "清明雨上.mp3", "温哥华悲伤一号.mp3", "漂洋过海来看你.mp3", "灌篮高手 - 只凝视着你（灌篮高手插曲）.mp3", "灌篮高手 - 好想大声叫喜欢你（灌篮高手片头曲）.mp3", "灌篮高手 - 直到世界尽头.mp3", "灰色头像.mp3", "爱你在心口难开.mp3", "爱的代价.mp3", "牛朝阳 - 爱情乞丐.mp3", "牧羊曲.mp3", "玻璃杯.mp3", "电台情歌.mp3", "痒.mp3", "痴心的我.mp3", "白狐.mp3", "相爱的泪水.mp3", "相见不如怀念.mp3", "离别.mp3", "秋天不回来.mp3", "空城.mp3", "突然的自我.mp3", "笑红尘.mp3", "筷子兄弟 - 祝福你亲爱的.mp3", "筷子兄弟 - 老男孩.mp3", "红色石头.mp3", "纯音乐 - 卡农.mp3", "纯音乐 - 圣母颂 (舒伯特).mp3", "美人吟.mp3", "羞答答的玫瑰静悄悄的开.mp3", "群星 - C大调第3弦奏鸣曲 中板.mp3", "群星 - 小抒情调 (巴赫).mp3", "老师你好.mp3", "肖邦 - 雨滴前奏曲.mp3", "胎教音乐 - 小溪和月亮的歌儿（C大调长笛与竖琴协奏曲）.mp3", "胎教音乐 - 布兰登堡协奏曲.mp3", "舒伯特 - 小夜曲（选自《天鹅之歌》）.mp3", "舒曼 - 梦幻曲（选自《童年情景》）.mp3", "英格兰民歌 - 你的秋波使我陶醉.mp3", "萨顶顶 - 万物生 (中文版).mp3", "蓝莲花.mp3", "西城男孩 - my love.mp3", "西海情歌.mp3", "触电.mp3", "说好的幸福呢.mp3", "谢军 - 那一夜.mp3", "谷村新司.-.风姿花传.mp3", "贝多芬 - 月光奏鸣曲.mp3", "贝多芬 - 给爱丽丝.mp3", "输了你赢了世界有如何.mp3", "迈克尔·杰克逊 - You Are Not Alone (Radio Edit).mp3", "追梦人.mp3", "那些花儿.mp3", "酸酸甜甜就是我.mp3", "铁齿铜牙纪晓岚.mp3", "镜子.面具.mp3", "门德尔松 - 乘着歌声的翅膀（钢琴版）.mp3", "陈奕迅_王菲 - 因为爱情.mp3", "陈小春 - 我爱的人.mp3", "陈楚生 - 倾国倾城.mp3", "雨蝶.mp3", "青春舞曲.mp3", "韩宝仪 - 粉红色的回忆.mp3", "风往北吹.mp3", "马友友 - 德弗札克：B小调大提琴协奏曲，作品104 第三乐章：终乐章.mp3", "鲁冰花.mp3", "黄小琥 - 白天不懂夜的黑（清晰版）.mp3", "黄昏.mp3"];//音乐列表
        this.playIndex = MyUtil.randomInt(this.musicList.length-1);
        this.audio;//html音频对象
        this.musicNameShow = this.musicList[this.playIndex];//音乐显示名称
        this.topDiv;//顶层DIV 用于包裹这个播放器 和访问这个播放器里的一些元素
        this.volume = obj.volume||0.2;//音量 默认0.2
        this.myInterval = null;

        if(this.isInitPaly){
            this.playOrPauseBtnInnerHtml = '暂停';
        }else{
            this.playOrPauseBtnInnerHtml = '播放';
        }
        //这里今后会添加对初始参数的处理和动态加载音频数据--只需添加爱相应的文件夹，或则文件也行
        //还有文件列表 会在前端 用于用户自由选择和显示
        //this._init();
        this._init.call(this);
    };
    MyMusicPlayer.prototype = {
        /**
         * 初始化
         */
        _init:function(){
            this._initView();
            this._initController();
        },
        /**
         * 初始化界面
         */
        _initView:function(){
            var topDiv =  this.topDiv = document.createElement('div');//创建顶层DIV
            /*音频对象的初始化 start*/
            var audio = this.audio = document.createElement('audio');//创建音频对象
            audio.controls = "controls";//控制权交给用户
            audio.volume = this.volume;//初始化音量

            audio.src = 'file:///E:/Music/'+ this.musicNameShow;
            /*音频对象的初始化 end*/

            /*按钮组的初始化 start*/
            var playOrPauseBtn = this.playOrPauseBtn = document.createElement('button');
            playOrPauseBtn.innerHTML = this.playOrPauseBtnInnerHtml;
            var forwardBtn = this.forwardBtn = document.createElement('button');
            forwardBtn.innerHTML = '上一曲';
            var againBtn = this.againBtn = document.createElement('button');
            againBtn.innerHTML = '重新听';
            var nextBtn = this.nextBtn = document.createElement('button');
            nextBtn.innerHTML ='下一曲';
            var randomBtn = this.randomBtn = document.createElement('button');
            randomBtn.innerHTML = '随机听';
            /*按钮组的初始化 end*/

            /*音乐名称显示div的初始化 start*/
            this.showMusicName = document.createElement('div');
            this.showMusicName.innerHTML = this.musicNameShow;
            /*音乐名称显示div的初始化 end*/

            //将以上元素添加到顶层IDV中
            topDiv.appendChild(audio);
            topDiv.appendChild(document.createElement('BR'));//换行
            topDiv.appendChild(this.showMusicName);
            //topDiv.appendChild(document.createElement('BR'));//换行
            topDiv.appendChild(playOrPauseBtn);
            topDiv.appendChild(forwardBtn);
            topDiv.appendChild(againBtn);
            topDiv.appendChild(nextBtn);
            topDiv.appendChild(randomBtn)


        },
        /**
         * 初始化控制层
         * @private
         */
        _initController:function(){
            if(this.isInitPaly){
                this.play();
            }
            /*按钮功能控制 start*/
            //暂停或播放按钮
            MyUtil.addClickEvent(this.playOrPauseBtn,function(){
                if(this.isInitPaly){
                    this.pause();
                }else{
                    this.play();
                }
            },this);
            //上一曲按钮
            MyUtil.addClickEvent(this.forwardBtn,function(){
                  this._play(-1);
            },this);
            //重新听
            MyUtil.addClickEvent(this.againBtn,function(){
                this._play(0);
            },this);
            //下一曲
            MyUtil.addClickEvent(this.nextBtn,function(){
               this._play(1);
            },this);

            MyUtil.addClickEvent(this.randomBtn,function(){
                this._play(MyUtil.randomInt(this.musicList.length-1),false);
            },this);

            /*按钮功能控制 end*/
        },
        /**
         * 播放
         */
        play:function(){
            this.playOrPauseBtn.innerHTML = '暂停';
            this.isInitPaly = true;
            this.showMusicName.innerHTML = this.musicNameShow;
            this.audio.play();
            var me = this;
            var isEnd = function(){
                if(me._isEnded()){
                    me._play(1);
                }
            }
            if(this.myInterval){clearInterval(this.myInterval);}
            this.myInterval = setInterval(isEnd,100);//每毫秒都监听该歌曲是否结束 如果结束则自动切换至下一首
        },
        /**
         * 暂停
         */
        pause:function(){
            this.playOrPauseBtn.innerHTML = '播放';
            this.isInitPaly = false;
            this.audio.pause();
        },
        /**
         * 内部调用的播放控制方法
         * @param {Number} index 索引值
         * @param {Boolean} flag 是否递增 如果递增 则累加  不是递增则为index 默认true
         * @private
         */
        _play:function(index,flag){
             var _flag  = flag ? false:true;
             if(_flag){//如果是递增
                 this.playIndex = this.playIndex + index;
             }else{//如果不是  这里暂时用于随机
                 this.playIndex = index;
             }

            if(this.playIndex<=0){//自动循环
                this.playIndex = this.musicList.length - 1;
            }else if(this.playIndex>=this.musicList.length-1){
                this.playIndex = 0;
            }

            this.musicNameShow = this.musicList[this.playIndex];
            this.audio.src = "file:///E:/Music/"+ this.musicNameShow;
            this.play();
        },
        /**
         * 是否播放结束
         * @return {Boolean}
         * @private
         */
        _isEnded:function(){
            return this.audio.ended;
        },
        /**
         * 渲染到页面元素为id的地方
         * @param id
         */
        render:function(id){
            document.getElementById(id).appendChild(this.topDiv);
        }
    };

    window.onload = function(){
        var myPlayer = new MyMusicPlayer();
        myPlayer.render("myMusicPlayer");

        var myPlayer2 = new MyMusicPlayer({isInitPlay:true});//这里测试添加初始化参数
        myPlayer2.render("myMusicPlayer2");
    }

</script>
<body>
<div id="myMusicPlayer"></div>
<div id="myMusicPlayer2"></div>
</body>
</html>
``````````

如代码所述：我将整体结构已函数的形式整理了下 使用时只需new个对象 将该对象渲染到某个id的html元素上 即可。

有关歌曲以及路径 可以根据您的磁盘放置情况 自己更改。sleep...

\----------------------------------------------------------------------------------------

失眠了，顺便查了下JS读取磁盘的方式，发现只有IE可以...其他不支持这种不安全方式啊...

//http://hi.baidu.com/elick/item/5a7fec0c40b680ce91571821
//http://heisetoufa.iteye.com/blog/342767

本来计划搞个格式为\[文件路径1:\['文件1','文件N'\],文件路径N:\['文件1','文件N'\]\]这样的读取格式，其中文件路径我只需输入 然后程序自动将文件读取加载到数组里呢

不过发现浏览器不支持的缘故，看来曲线救国的方式就是使用后端语言先生成个这种格式的文件，然后读取这个文件...

当然借助JS框架也是可行的。






[VBEY-QAEA-7NMI.jpg]: /pro/os/crawler/VBEY-QAEA-7NMI.jpg