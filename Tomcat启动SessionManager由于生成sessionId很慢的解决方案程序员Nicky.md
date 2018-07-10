---
title: Tomcat启动SessionManager由于生成sessionId很慢的解决方案
date: 2018-05-23 18:07:47
categories: "开发"
tags:
	- 文本编辑器
	- Java
	- Tomcat
	- 编程语言
	- Vim

---

日志：


`org.apache.catalina.startup.HostConfig.deployDirectory``Deploying web application directory [/opt/service/tomcat8.5.16/webapps/manager]，` org.apache.catalina.startup.Catalina.start Server startup in 34487 ms

原因：

tomcat在启动的时候session引起的随机数问题导致的。Tocmat的Session ID是通过SHA1算法计算得到的。

解决方案：

方案一：修改JDK中的文件java.security

拓展：查看jdk安装目录

which java

``````````
vim $JAVA_HOME/jre/lib/security/java.securitysecurerandom.source=file:/dev/random改为securerandom.source=file:/dev/urandom
``````````

方案二：修改tomcat中的catalina.sh

``````````
vim $TOMCAT_HOME/bin/catalina.shif [[ "$JAVA_OPTS" != *-Djava.security.egd=* ]]; then JAVA_OPTS="$JAVA_OPTS -Djava.security.egd=file:/dev/urandom"fi
``````````

![Tomcat启动SessionManager由于生成sessionId很慢的解决方案][Tomcat_SessionManager_sessionId]


[Tomcat_SessionManager_sessionId]: /pro/os/crawler/ZYJZ-3AUN-EQMM.jpg
 *  **原文作者：** 程序员Nicky
 *  **原文链接：** https://www.toutiao.com/item/6558716000190071304/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。