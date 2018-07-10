---
title: 一款APP设计的从0到1之：Android设计规范篇
date: 2017-08-29 22:31:39
categories: "开发"
tags:
	- 软件
	- 设计师
	- 笔记本电脑
	- iOS
	- 三星

---

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android]

之前U妹给大家分享了《一款APP设计的从0到1之：iOS精华篇》，今天U妹给大家带来的是Android的设计规范篇。

iOS设计规范回顾：

《一款APP设计的从0到1之：iOS精华篇》

Android的设计规范不同于iOS，Android是一个开源的系统，国内外有很多的手机厂商，这就导致了有非常多的Android机型，如小米、华为、魅族、三星等，每一家都有自己的操作系统，都有一套自己的UI设计规范。

U妹列了一个小小的目录：

**一、基础概念**

**二、Android界面设计规范**

**三、Android切图标注**

**四、安卓开发单位换算**

**五、总结**

**一、基础概念**

**1. 什么是DPI？**

**DPI**（Dots Per Inch）：每英寸点数，表示指屏幕密度。是测量空间点密度的单位，最初应用于打印技术中，它表示每英寸能打印上的墨滴数量。较小的DPI会产生不清晰的图片。

后来DPI的概念也被应用到了计算机屏幕上，计算机屏幕一般采用PPI（Pixels Per Inch）来表示一英寸屏幕上显示的像素点的数量，现在DPI也被引入。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 1]

安装Windows操作系统的电脑屏幕PPI的初始值是96，Mac的初始值是72，虽然这个值从80年代起就不是很准确了。 一般来说，非retina桌面（包括Mac）的PPI的取值区间在72-120之间，因为这个取值区间能够确保你的作品在任何地方都能保持大致相同的比例。

这里有一个应用实例： 27寸Mac影院显示屏的PPI是109，这表示在每英寸的屏幕上显示了109个像素点。斜角长是25.7英寸（65cm），实际屏幕的宽度大概是23.5英寸，23.5109约等于2560，因此原始屏幕分辨率就是2560x1440px。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 2]

**屏幕密度计算公式：**

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 3]

**1080x1920px屏幕密度：**

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 4]

**2. 什么是PPI？**

**PPI**(Pixels Per Inch)：图像分辨率；是每英寸图像内有多少个像素点，分辨率的单位为ppi，通常叫做像素每英寸。图像分辨率一般被用于ps中，用来改变图像的清晰度。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 5]

**二、Android界面设计规范**

**1. Android各设备屏幕密度**

安卓尺寸众多，按每个屏幕去适配肯定是不现实的。

所以为了解决这个问题，安卓手机屏幕有自己初始的固定密度，安卓会根据这些屏幕不同的密度自己进行适配。这一点内容掌握到能满足自己设计工作需要就可以了……

以下是Android的密度划分以及代表的分辨率，这里你可以发现已经和设计稿尺寸和切图输出开始挂钩了。

**安卓各屏幕密度**

<table>
 <tbody>
  <tr>
   <td><strong>密度</strong></td>
   <td>LDPI</td>
   <td>MDPI</td>
   <td>HDPI</td>
   <td>XHDPI</td>
   <td>XXHDPI</td>
   <td>XXXHDPI</td>
  </tr>
  <tr>
   <td><strong>密度数</strong></td>
   <td>120</td>
   <td>160</td>
   <td>240</td>
   <td>320</td>
   <td>480</td>
   <td>640</td>
  </tr>
  <tr>
   <td><strong>分辨率</strong></td>
   <td>240X320</td>
   <td>320X480</td>
   <td>480X800</td>
   <td>720X1280</td>
   <td>1080X1920</td>
   <td>3840X2160</td>
  </tr>
  <tr>
   <td><strong>倍数关系</strong></td>
   <td>0.75x</td>
   <td>1X</td>
   <td>1.5X</td>
   <td>2X</td>
   <td>3X</td>
   <td>4X</td>
  </tr>
  <tr>
   <td><strong>px、dp、sp的关系</strong></td>
   <td>1dp=1px</td>
   <td>1dp=1.5px</td>
   <td>1dp=1.5px</td>
   <td>1dp=2px</td>
   <td>1dp=3px</td>
   <td>1dp=4px</td>
  </tr>
  <tr>
   <td><strong>市场比</strong></td>
   <td>---</td>
   <td>★</td>
   <td>★★</td>
   <td>★★★★</td>
   <td>★★★★★</td>
   <td>★</td>
  </tr>
 </tbody>
</table>

**U妹来带大家了解一下iPhone各设备的手机屏幕密度：**

**iphone 4/4S/5/5S/SE/6/7≈320DPI**

**2. Android开发单位DP和SP**

**DP：**安卓专用长度单位。以160 DPI屏幕为标注，则1DP=1PX

计算公式：dp x dpi/160=px

例：以720x1280px （320dpi）为例计算 1dp x 320 dpi/=2px

**SP：**安卓专用字体单位。以160 DPI屏幕为标注，则1SP=1PX

计算公式：sp x dpi/160=px

例：以720x1280px （320dpi）为例计算 1sp x 320 dpi/=2px

**3. 设计稿尺寸**

从目前市场主流设备尺寸来看，我们要用**1080 x 1920 PX** 来做安卓设计稿尺寸。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 6]

**以1080x1920px作为设计稿标准尺寸的原由：**

① 从中间尺寸向上和向下适配的时候界面调整的幅度最小，最方便适配。

② 大屏幕时代依然以小尺寸作为设计尺寸，会限制设计师的设计视角。

③ 用主流尺寸来做设计稿尺寸，极大的提高了视觉还原和其他机型适配。

所以做安卓设计稿时请以**1080x1920px**来做设计稿

（sketch用户以540 x 960来做设计稿）

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 7]

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 8]

**界面设计控件尺寸：**

<table>
 <tbody>
  <tr>
   <td><strong>分辨率</strong></td>
   <td>DPI</td>
   <td>状态栏高度</td>
   <td>导航栏高度</td>
   <td>标签栏高度</td>
  </tr>
  <tr>
   <td><strong>720x1280</strong></td>
   <td>XHDPI</td>
   <td>50px</td>
   <td>96px</td>
   <td>96px</td>
  </tr>
  <tr>
   <td><strong>1080x1920</strong></td>
   <td>XXHDPI</td>
   <td>60px</td>
   <td>144px</td>
   <td>150px</td>
  </tr>
 </tbody>
</table>

**4. 安卓图标尺寸**

<table>
 <tbody>
  <tr>
   <td><strong>屏幕大小</strong></td>
   <td>启动图标</td>
   <td>操作栏图标</td>
   <td>上下文图标</td>
   <td>系统通知图标</td>
   <td>最细画笔</td>
  </tr>
  <tr>
   <td><strong>320x480</strong></td>
   <td>48x48px</td>
   <td>32x32px</td>
   <td>16x16px</td>
   <td>24x24px</td>
   <td>不小于2px</td>
  </tr>
  <tr>
   <td><p><strong>480x800</strong></p><p><strong>480x854</strong></p></td>
   <td>72x72px</td>
   <td>48x48px</td>
   <td>24x24px</td>
   <td>36x36px</td>
   <td>不小于3px</td>
  </tr>
  <tr>
   <td><strong>720x1280</strong></td>
   <td>96x96px</td>
   <td>64x64px</td>
   <td>32x32px</td>
   <td>48x48px</td>
   <td>不小于4px</td>
  </tr>
  <tr>
   <td><strong>1080x1920</strong></td>
   <td>144x144px</td>
   <td>96x96px</td>
   <td>48x48px</td>
   <td>72x72px</td>
   <td>不小于6px</td>
  </tr>
 </tbody>
</table>

安卓的图标相对iOS来说较少，我们只需提供一下几个尺寸就可以了，但是需要提高2套，圆角和直角各一套，因为有的地方都会用到。

**512x512px** 

**192x192px** 

**144x144px** 

**96x96px** 

**72x72px** 

**48x48px** 

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 9]

因为安卓有很多的机型，不同屏幕密度的手机对应的icon大小也是不同的，所以U妹这里没法给你给出相应的icon所被应用的位置。

**5. 安卓设计字体**

英文字体为 **Roboto字体**

中文字体为 **思源黑体**。在在Android 5.0之后，使用的是思源黑体，字体文件有2个名称，“source han sans”和“noto sans CJK”。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 10]

思源黑体是Adobe和Google领导开发的开源字体，支持繁简日韩，有7种字体粗细。

**思源黑体字体下载地址**，请戳这里《近100+个免费可商用字体大集合（附字体包）》

**6. 常见主流手机尺寸和分辨率**

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 11]

**三、Android切图标注**

**1. 标注设计稿时，使用px还是dp或sp？**

答：这个问题需要和安卓工程师沟通，推荐使用dp和sp进行标注（这里指的是在安卓设计稿的前提下）。但目前很多设计师对dp和sp这个单位并不理解，所以有些设计师提供安卓设计稿的时候依旧使用px进行标注，这一点去和你的搭档工程师进行沟通，如果不影响他开发以及他能换算清楚的前提下，你可以考虑使用Px，但是我并不推荐。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 12]

这里要记住一点(你只需要记住能帮助你工作就可以)：

**当屏幕密度为MDPI（160DPI）时，1dp=1px** 

**当屏幕密度为MDPI（160DPI）时，1sp=1px**

**像素字号=屏幕密度/160 \* sp字号** 可以根据这个去算算设计稿中的像素字号标注为sp是多少，比如xHDPI下,36px的字标注为sp就是18sp，以此类推。

按照不同的屏幕密度换算，也就是下图所示的意思：

**2. 你需要提供几套切图资源？**

**答：**理论状态下，如果你想兼顾到目前还存在的各个机型，应该为不同的密度提供不同尺寸大小的切图。

但这无疑提升了巨大的工作量，而且还可能浪费很大的资源空间，实际上，很多机型已经不占有主流市场了，而且很多奇葩的分辨率也没必要去考虑适配，所以，具体输出几套需要看公司的产品需求而定。

**通常我是这么干的：**

**![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 13]**

选取最大尺寸提供一套切图资源，交给开发工程师处理，适配到各个屏幕密度。

这里要注意，这个“最大尺寸”，指的并不是目前市面上Android手机出现过的最大尺寸，而是指目前流行的主流机型中的最大尺寸，这样可节省很大的资源空间。关于最大尺寸选取多少，你需要和你们的安卓工程师沟通，每个安卓工程师对这个问题的结论并不同。（我的安卓搭档，让我**提供XXHDPI的切图资源就好，我用的切图工具是Cutterman，切图一键搞定**）

**切图神器Cutterman下载地址：**《真正解放你双手的切图神器》

**3. Android的切图资源提供哪个尺寸给开发哥哥？**

**答：**iOS的切图有@2x，@3x之分，那么Android的切图根据dpi的不同，其实和iOS的类似，只不过是按照dpi来进行资源文件夹的命名，如下图：

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 14]

根据不同的分辨率进行切图归类，但是你看到了，如果切片特别多，提供5套切图岂不是要累死了？

一般情况下，我们只需要提供3套切图资源就可以满足安卓工程师的适配，分别是**HDPI、XHDPI、 XXHDPI** 3套切图资源**。**

目前我使用的办法就是只提供最大尺寸的切图，交给安卓工程师自己去缩放适配其他分辨率吧，所以和你的搭档沟通一下。

其实现在绝大多数公司限于人力物力的限制，没有这么严格的工作方式，基本上就是一个文件夹，命名好了就提供给工程师了。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 15]

这里还是提醒各位，没有固定的工作方式和方法，任何方式都是为了提升工作效率而进行的。

**4. 在做设计稿时我们遇到的最多问题**

**① 用哪种尺寸做设计稿？**

**iOS：**用750x1334px来做设计稿。

**安卓：**就目前的市场来看，XXHDPI属于主流机型；这样无论是标注，还是主流机型都能兼顾的到，所以推荐使用**1080x1920px**来做设计稿尺寸，这样即使你标注的是px，工程师也可以很方便的进行换算。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 16]

**② 如何用iOS的设计稿去适配安卓（划重点啦）**

现在有一种情况现在非常普遍，那就是**一稿两用**；设计师都是做IOS版本的设计稿，来适配安卓，现在要给安卓用，应该怎么办？

iPhone的屏幕密度已经达到xHDPI了，用750x1334px的尺寸做设计稿；

实际上，**750x1334的@3x的切图资源正好是安卓XXhdpi（1080x1920px）的切图资源**；安卓开发用iOS的设计稿自己进行换算就可以了，前提是你必须和安卓工程师沟通。

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 17]

另一种情况：你可以把750x1334的设计稿等比例调整尺寸到安卓1080x1920尺寸下，对各个控件进行微调，重新提供标注（用dp标注）。也就是说，你需要提供两套标注，一套给iOS的标注，一套给Android的标注。

**③ 大家可能还有一个问题，那就是我用cutterman切安卓图片输出的有drawable和mipmap 2个文件夹，到底将哪个给开发工程师呢？**

![一款APP设计的从0到1之：Android设计规范篇][APP_0_1_Android 18]

答：以前用的开发工具，是只有drawable， 没有mipmap的，后来新的开发工具里面才有mipmap这个文件夹，专门用来放png格式的图片的，不过drawable里面还是可以放png图片。

所以现在我们**给安卓工程师的切图输出文件只需给mipmap-前缀开头的就好**。

**四、Android开发单位换算**

**1. 安卓机型各种尺寸下的PX与DP、SP的对应关系**

<table>
 <tbody>
  <tr>
   <td><strong>密度</strong></td>
   <td>分辨率</td>
   <td>当1dp对应的 px</td>
   <td>当1sp对应的px</td>
  </tr>
  <tr>
   <td><strong>LDPI</strong></td>
   <td>240x320px 120dpi</td>
   <td>1dp=0.75px</td>
   <td>1sp=0.75px</td>
  </tr>
  <tr>
   <td><p><strong>MDPI</strong></p></td>
   <td>320x480px 160dpi</td>
   <td>1dp=1px</td>
   <td>1sp=1px</td>
  </tr>
  <tr>
   <td><strong>HDPI</strong></td>
   <td>480x800px 240dpi</td>
   <td>1dp=1.5px</td>
   <td>1sp=1.5px</td>
  </tr>
  <tr>
   <td><strong>XHDPI</strong></td>
   <td>720x1280px 320dpi</td>
   <td>1dp=2px</td>
   <td>1sp=2px</td>
  </tr>
  <tr>
   <td><strong>XXHDPI</strong></td>
   <td>1080x1920px 480dpi</td>
   <td>1dp=3px</td>
   <td>1sp=3px</td>
  </tr>
 </tbody>
</table>

**2. 字体单位SP与PX的对应关系**

<table>
 <tbody>
  <tr>
   <td><strong>字体大小单位</strong></td>
   <td>在320dpi下</td>
   <td>1sp=2px</td>
   <td></td>
   <td></td>
   <td></td>
   <td></td>
   <td></td>
  </tr>
  <tr>
   <td><strong>字体大小</strong></td>
   <td>36px</td>
   <td>32px</td>
   <td>30px</td>
   <td>28px</td>
   <td>26px</td>
   <td>24px</td>
   <td>22px</td>
  </tr>
  <tr>
   <td><strong>字体大小</strong></td>
   <td>18sp</td>
   <td>16sp</td>
   <td>15sp</td>
   <td>14sp</td>
   <td>13sp</td>
   <td>12sp</td>
   <td>11sp</td>
  </tr>
 </tbody>
</table>

**3. 距离单位DP与PX的对应关系**

<table>
 <tbody>
  <tr>
   <td><strong>单位dp</strong></td>
   <td>8dp</td>
   <td>16dp</td>
   <td>24dp</td>
   <td>32dp</td>
   <td>48dp</td>
   <td>72dp</td>
  </tr>
  <tr>
   <td><strong>240x320px</strong></td>
   <td>6px</td>
   <td>12px</td>
   <td>18px</td>
   <td>24px</td>
   <td>36px</td>
   <td>54px</td>
  </tr>
  <tr>
   <td><strong>320x480px</strong></td>
   <td>8px</td>
   <td>16px</td>
   <td>24px</td>
   <td>32px</td>
   <td>48px</td>
   <td>72px</td>
  </tr>
  <tr>
   <td><strong>480x800px</strong></td>
   <td>12px</td>
   <td>24px</td>
   <td>36px</td>
   <td>48px</td>
   <td>72px</td>
   <td>108px</td>
  </tr>
  <tr>
   <td><strong>720x1280px</strong></td>
   <td>16px</td>
   <td>32px</td>
   <td>48px</td>
   <td>64px</td>
   <td>96px</td>
   <td>144px</td>
  </tr>
  <tr>
   <td><strong>1080x1920px</strong></td>
   <td>24px</td>
   <td>48px</td>
   <td>72px</td>
   <td>96px</td>
   <td>144px</td>
   <td>216px</td>
  </tr>
 </tbody>
</table>

**五、总结**

关于《一款APP设计的从0到1之：安卓设计规范篇》就全部讲完了，希望可以给你有很大的帮助；U妹这里说的只是一种工作方法，好的工作方法才能自己事半功倍，在具体工作中也要灵活应用，一定要多和开发沟通交流，良好的沟通才是解决问题的唯一方法，有疑问题也可给U妹留言，我们下期再见！


[APP_0_1_Android]: /pro/os/crawler/UBEA-RYVR-VYRN.jpg
[APP_0_1_Android 1]: /pro/os/crawler/BIRI-YMZF-A2AU.jpg
[APP_0_1_Android 2]: /pro/os/crawler/7NYZ-RJJF-2YQ2.jpg
[APP_0_1_Android 3]: /pro/os/crawler/N77J-6RUF-6Z63.jpg
[APP_0_1_Android 4]: /pro/os/crawler/7JNF-A3YR-JVN2.jpg
[APP_0_1_Android 5]: /pro/os/crawler/REQU-BIQU-UMMI.jpg
[APP_0_1_Android 6]: /pro/os/crawler/II7F-FNBY-MF3U.jpg
[APP_0_1_Android 7]: /pro/os/crawler/QRE6-32QR-I2EQ.jpg
[APP_0_1_Android 8]: /pro/os/crawler/Y2M3-MNRE-U2Y2.jpg
[APP_0_1_Android 9]: /pro/os/crawler/YVEE-6J7N-2MV2.jpg
[APP_0_1_Android 10]: /pro/os/crawler/YVIR-YR6F-2UBE.jpg
[APP_0_1_Android 11]: /pro/os/crawler/FIZB-BAAV-VVQA.jpg
[APP_0_1_Android 12]: /pro/os/crawler/ABNR-IMZE-NUVN.jpg
[APP_0_1_Android 13]: /pro/os/crawler/UINF-7JVU-BANE.jpg
[APP_0_1_Android 14]: /pro/os/crawler/N2UR-FYER-URYJ.jpg
[APP_0_1_Android 15]: /pro/os/crawler/B2E2-UIYE-JR7N.jpg
[APP_0_1_Android 16]: /pro/os/crawler/NYZM-2EIA-YR7N.jpg
[APP_0_1_Android 17]: /pro/os/crawler/MIFI-7NIM-Q6FU.jpg
[APP_0_1_Android 18]: /pro/os/crawler/FVYR-E2AE-R7FU.jpg
 *  **原文作者：** UI妹儿
 *  **原文链接：** https://www.toutiao.com/item/6459704252653650445/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。