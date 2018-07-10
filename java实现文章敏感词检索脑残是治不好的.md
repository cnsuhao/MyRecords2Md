---
title: java实现文章敏感词检索
date: 2017-08-09 11:21:33
categories: "开发"
tags:
	- Java
	- Word
	- 文章
	- GitHub
	- 编程语言

---

import javax.annotation.PostConstruct;  


import java.util.\*;

/\*\*

\* 利用三叉树原理高效检查文中敏感词

\* Created by kakasun on 2017/7/4.

\*/

public class CreateSensitiveWordsLib \{

/\*\*

\* 敏感词库

\*/

static Map<Character,Node> SensitiveWordsLib = new HashMap<Character,Node>();

@PostConstruct

public void init()\{

updateSensitiveWordsLib();

\}

/\*\*

\* 更新敏感词字典

\*/

public void updateSensitiveWordsLib()\{

Map<Character,Node> lib = new HashMap<Character,Node>();

//敏感词应从数据库获取

List<String> words = Arrays.asList("2b","你大爷的","怂","他妈的","屌","我他妈","你他妈");

for (String s:words)\{

add(s,lib);

\}

synchronized (SensitiveWordsLib)\{

SensitiveWordsLib.clear();

SensitiveWordsLib = lib;

\}

\}

/\*\*

\* 检查出文字中的所有敏感词

\* @param text

\* @return

\*/

public Set<String> checkOutAllSensitiveWord(String text,boolean isHtml)\{

Set<String> set = new TreeSet<String>();

if (text == null || text.length() == 0)\{

return set;

\}

//过滤标签

if(isHtml)\{

text = text.replaceAll("</?\[^<>\]+>","");

\}

//过滤换行符

text = text.replaceAll("\[ \]+","");

int l = text.length();

String word;

for(int i = 0;i < l;i++)\{

word = checkOutSensitiveWord(i,text);

if(word != null)\{

set.add(word);

\}

\}

return set;

\}

/\*\*

\* 从传入的下标检查传入的字符串的敏感词

\* @param index

\* @param text

\* @return

\*/

public String checkOutSensitiveWord(int index,String text)\{

if (text == null || text.length() == 0)\{

return null;

\}

char c;

Map<Character,Node> lib ;

synchronized (SensitiveWordsLib)\{

lib = SensitiveWordsLib;

\}

if (lib == null)\{

return null;

\}

Node node;

StringBuffer sb = new StringBuffer();

for (;index < text.length();index++)\{

c = text.charAt(index);

node = lib.get(c);

if (node == null)\{

return null;

\}

sb.append(node.getCurrenChar());

/\*

当“你他妈”和“你他妈的”都是敏感词,

检查到“你他妈”这个敏感词就返回，不再继续检查

\*/

if (node.isEnd())\{

return sb.toString();

\}

lib = node.getChildren();

\}

return null;

\}

/\*\*

\* 向默认字典中添加敏感词

\* @param word

\*/

public void add(String word)\{

add(word,SensitiveWordsLib);

\}

/\*\*

\* 向字典中添加敏感词

\* @param word

\* @param lib

\*/

void add(String word,Map<Character ,Node> lib)\{

Map<Character ,Node> current = lib;

char c;

Node node;

int l = word.length() -1;

for (int i = 0;i <= l ; i++)\{

c = word.charAt(i);

node = current.get(c);

if (node == null)\{

node = new Node();

node.setCurrenChar(c);

current.put(c,node);

if (i == l)\{

node.setEnd(true);

\}else \{

node.setHasChildren(true);

current = node.getChildren();

\}

continue;

\}

/\*

当“你他妈”和“你他妈的”都是敏感词可以忽略“你他妈的”这个敏感词

只检查“你他妈”这个敏感词

\*/

if(node.isEnd())\{

return;

\}

current = node.getChildren();

\}

\}

/\*\*

\* 敏感词节点

\*/

class Node\{

/\*\*

\* 当前字符

\*/

char currenChar;

/\*\*

\* 是否是铭感词的结束字符

\*/

boolean end = false;

/\*\*

\* 是否有子节点

\*/

boolean hasChildren = false;

/\*\*

\* 该节点的分支子节点

\*/

Map<Character,Node> children;

public char getCurrenChar() \{

return currenChar;

\}

public void setCurrenChar(char currenChar) \{

this.currenChar = currenChar;

\}

public boolean isEnd() \{

return end;

\}

public void setEnd(boolean end) \{

this.end = end;

\}

public boolean isHasChildren() \{

return hasChildren;

\}

public void setHasChildren(boolean hasChildren) \{

this.hasChildren = hasChildren;

\}

public Map<Character, Node> getChildren() \{

if (children == null)\{

children = new HashMap<Character,Node>();

\}

return children;

\}

\}

public static void main(String\[\] arg0)\{

CreateSensitiveWordsLib lib = new CreateSensitiveWordsLib();

lib.init();

String testStr = "我他妈那么爱你。 你他妈爱理不理。 我他妈对你放电。 你他妈装没看见。 你他妈喝着红牛。 我他妈口水直流。" +

" 我他妈抽一根烟。 你他妈让我靠边。 我他妈喝一口酒。 你他妈扭头就走。 我他妈牵你的手。 你他妈浑身发抖。" +

" 我他妈跟你接吻。 你他妈死活不肯。 你他妈无情无意。 我他妈决定放弃。 我他妈刚要跳楼。 你他妈才回过头。 " +

"你他妈回心转意。 我他妈刚好落地。 你他妈哭得像鬼 . 我他妈死的后悔 . 大海啊，他妈尽是水. 蜈蚣啊，他妈尽是腿 . " +

"辣椒啊，尽他妈辣嘴 . 有你们我他妈从不后悔.你个2b，你他妈真怂。你大爷的。我去你大爷的。";

Set<String> set = lib.checkOutAllSensitiveWord(testStr,false);

System.out.println(Arrays.toString(set.toArray()));

\}

\}

敏感词检查的核心代码已分享到github上。

https://github.com/yikeso/sejar/blob/master/src/main/java/com/china/ciic/se/sejar/CreateSensitiveWordsLib.java
 *  **原文作者：** 脑残是治不好的
 *  **原文链接：** https://www.toutiao.com/item/6452109864209482253/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。