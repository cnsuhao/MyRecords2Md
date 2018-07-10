---
title: 享元模式java + javascript
date: 2013-04-27 21:10:53
categories: "开发"
tags:
	- java

---

**享元模式**（英语：Flyweight Pattern）是一种软件[设计模式][Link 1]。它使用共享物件，用来尽可能减少内存使用量以及分享资讯给尽可能多的相似物件；它适合用于当大量物件只是重复因而导致无法令人接受的使用大量内存。通常物件中的部分状态是可以分享。常见做法是把它们放在外部数据结构，当需要使用时再将它们传递给享元。

典型的享元模式的例子为[文书处理器][Link 2]中以图形结构来表示字符。一个做法是，每个[字形][Link 3]有其字型外观, 字模 metrics, 和其它格式资讯，但这会使每个字符就耗用上千字节。取而代之的是，每个字符参照到一个共享字形物件，此物件会被其它有共同特质的字符所分享；只有每个字符（文件中或页面中）的位置才需要另外储存。

## 示例 ##

### [Java][] ###

以下程式用来解释上述的文字。这个例子用来解释享元模式利用只加载执行任务时所必需的最少资料，因而减少内存使用量。

``````````
public enum FontEffect {
    BOLD, ITALIC, SUPERSCRIPT, SUBSCRIPT, STRIKETHROUGH
}
 
public final class FontData {
    /**
     * A weak hash map will drop unused references to FontData.
     * Values have to be wrapped in WeakReferences, 
     * because value objects in weak hash map are held by strong references.
     */
    private static final WeakHashMap<FontData, WeakReference<FontData>> FLY_WEIGHT_DATA =
        new WeakHashMap<FontData, WeakReference<FontData>>();
    private final int pointSize;
    private final String fontFace;
    private final Color color;
    private final Set<FontEffect> effects;
 
    private FontData(int pointSize, String fontFace, Color color, EnumSet<FontEffect> effects) {
        this.pointSize = pointSize;
        this.fontFace = fontFace;
        this.color = color;
        this.effects = Collections.unmodifiableSet(effects);
    }
 
    public static FontData create(int pointSize, String fontFace, Color color,
        FontEffect... effects) {
        EnumSet<FontEffect> effectsSet = EnumSet.noneOf(FontEffect.class);
        for (FontEffect fontEffect : effects) {
            effectsSet.add(fontEffect);
        }
        // We are unconcerned with object creation cost, we are reducing overall memory consumption
        FontData data = new FontData(pointSize, fontFace, color, effectsSet);
 
        // Retrieve previously created instance with the given values if it (still) exists
        WeakReference<FontData> ref = FLY_WEIGHT_DATA.get(data);
        FontData result = (ref != null) ? ref.get() : null;
 
        // Store new font data instance if no matching instance exists
        if (result == null) {
            FLY_WEIGHT_DATA.put(data, new WeakReference<FontData> (data));
            result = data;
        }
        // return the single immutable copy with the given values
        return result;
    }
 
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof FontData) {
            if (obj == this) {
                return true;
            }
            FontData other = (FontData) obj;
            return other.pointSize == pointSize && other.fontFace.equals(fontFace)
                && other.color.equals(color) && other.effects.equals(effects);
        }
        return false;
    }
 
    @Override
    public int hashCode() {
        return (pointSize * 37 + effects.hashCode() * 13) * fontFace.hashCode();
    }
 
    // Getters for the font data, but no setters. FontData is immutable.
}




 JavaSciript:
``````````

``````````
<script type="text/javascript">
  <!--
	var obj = {
	     setCom : function(com){
		      this.com = com;
		 },
		 getName : function(){
		     return this.com.getName();
		 },
		 setName : function(name){
		     this.com.setName(name);
		 }
	};
	var getPack = function(com){
	    obj.setCom(com);
		return obj;
	}
	var person1 ={
	    name : '张三',
		setName : function(name){
		    this.name = name;
		},
		getName : function(){
		    return this.name;
		}
	};
	var person2 ={
	    name : '李四',
		setName : function(name){
		    this.name = name;
		},
		getName : function(){
		    return this.name;
		}
	};
	var p1 = getPack(person1);
	p1.setName('张三2');
	var a1 = p1.com;
	
	var p2 = getPack(person2);
	var a2 = p2.com;
	//p2.setName('李四2');
	alert(p1 === p2+"---"+a1 === a2);
	alert(person1.name+""+person2.name);
  //-->
  </script>
``````````




``````````

``````````

``````````
-----------------------
``````````

感谢永泰同志！






[Link 1]: https://zh.wikipedia.org/wiki/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F_%28%E8%AE%A1%E7%AE%97%E6%9C%BA%29
[Link 2]: https://zh.wikipedia.org/wiki/%E6%96%87%E6%9B%B8%E8%99%95%E7%90%86%E5%99%A8
[Link 3]: https://zh.wikipedia.org/wiki/%E5%AD%97%E5%BD%A2
[Java]: https://zh.wikipedia.org/wiki/Java