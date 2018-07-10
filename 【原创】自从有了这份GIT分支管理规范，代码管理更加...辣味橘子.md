---
title: 【原创】自从有了这份GIT分支管理规范，代码管理更加...
date: 2018-04-04 10:14:53
categories: "开发"
tags:
	- Git
	- 技术
	- Gradle

---

![【原创】自从有了这份GIT分支管理规范，代码管理更加...][GIT_...]

GIT，作为目前最流行的代码版本管理工具，已经使用在了绝大多数开发团队中。甚至很多还在使用SVN进行代码管理的团队，也在依照GIT代码管理的思想来使用SVN。

这里不介绍GIT的底层命令是如何使用，只谈谈GIT在代码管理上的一个使用流程规范。

马上上干货。。。

![【原创】自从有了这份GIT分支管理规范，代码管理更加...][GIT_... 1]

1.  MASTER总是和线上部署的代码保持一致，例如，当前版本为1.2.5
2.  一个开发迭代开始，出现了3个特性任务，A，B，C。这时从Master分支新建3个feature分支，命名为feature/A，feature/B，feature/C
3.  迭代开发阶段结束后，A和B特性开发任务完成，并准备测试和上线，特性C将推迟，不提交测试和上线
4.  从Master分支新建分支release/1.3.0， 将feature/A和feautre/B分支的代码合并到分支release/1.3.0，删除feature/A和feautre/B分支
5.  分支release/1.3.0进行测试，出现了bug1和bug2，那么从分支release/1.3.0新建分支bugfix/bug1和bugfix/bug2
6.  bug1修复后分支bugfix/bug1合并到release/1.3.0，删除分支bugfix/bug1；此时出现了bug3，那么从分支release/1.3.0新建分支bugfix/bug3
7.  转到 第5步骤 直到bug修复可以达到上线标准
8.  release/1.3.0分支作为发布版本进行发布
9.  上线后，release/1.3.0分支合并到MASTER；删除release/1.3.0分支；MASTER分支合并到所有的其他feature分支（feature/C）和bug分支（bugfix/bugN）
10. 进入开发下一个迭代，修复剩余bug和继续开发特性C
11. 线上发生紧急bugX，那么从MASTER新建分支hotfix/bugX，并修复
12. 从Master分支新建分支release/1.3.1，将分支hotfix/bugX合并到release/1.3.1，测试并上线
13. 上线后，release/1.3.1分支合并到MASTER；删除release/1.3.1分支；MASTER分支合并到所有的其他feature分支（feature/C）和bug分支（bugfix/bugN）
14. 转到 第3步骤 或者 转到 第11步骤

版本号可以配合maven/gradle中的版本号设置。


整个管理流程适合多人同时开发，如果是单人开发可以相应的减少流程中的步骤以达到更加高效的目的。

尝试过这套流程的团队都说代码管理更加清晰了，你觉得呢？

![【原创】自从有了这份GIT分支管理规范，代码管理更加...][GIT_... 2]

# 图来自网络，版权归原作者 #


[GIT_...]: /pro/os/crawler/RY67-ZRME-UJJR.jpg
[GIT_... 1]: /pro/os/crawler/JJMI-BBB7-ZBEJ.jpg
[GIT_... 2]: /pro/os/crawler/BVJE-E2QR-JIMR.jpg
 *  **原文作者：** 辣味橘子
 *  **原文链接：** https://www.toutiao.com/item/6540405241077039620/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。