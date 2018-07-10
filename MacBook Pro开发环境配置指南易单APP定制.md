---
title: MacBook Pro开发环境配置指南
date: 2018-05-21 15:49:38
categories: "开发"
tags:
	- 文本编辑器
	- 编程语言
	- Mac
	- Zsh
	- Python

---

本文章主要记录新购Mac，需要安装的必备软件，由于有多台Mac，用途不一样。

 *  公司主力开发电脑
 *  家中主力开发电脑

公司主力开发电脑，主要功能是开发公司软件研发有关。而且有一些私有的东西，需要符合公司规范。

家中主力开发电脑，主要��与开源社区开发以及个人创作，涉及社区和个人创作内容，软件栈相对自由。

故此，记录一下Mac做为主力开发程序电脑，必备提升效率软件利器，工具选得好，下班下得早。

安装Homebrew包管理工具

Homebrew 是Mac OS 下的包管理工具，类似于Ubuntu下的apt-get命令，通过这个工具我们可以快速获取所需要的软件而不需要像在Windows系统中那样打开浏览器，找到需要下载的安装包，然��才能进行下载。Homebrew拥有安装、卸载、更新、查看、搜索等很多实用的功能。通过一条简单的指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

执行如命令安装：

``````````
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
``````````

稍等片刻，看到`successful`说明安装成功，具体根据所处网络速度而定。

利用brew安装软件��试：

\[1\]、安装wget工具

``````````
brew install wget
``````````

\[2\]、安装git工具

``````````
brew install git
``````````

通过brew把我们平时使用的命令行工具都安装上，喜欢Mac的原因就是可以提供类似Unix/Linux体验，并且有简洁美观的界面设计。

工作空间

此处基本都是个人喜好，我个人比较喜欢控制，所以对工作空间有一些自己的规范。

对于Mac系统，我通常会在根目录下建立`/data`用来��为创作空间。

``````````
sudo mkdir /datasudo chown xujiang:staff /data
``````````

如上，创建`/data`目录，并且授权给`xujiang`用户可以完全控制此目录，这里使用了`sudo`越权操作，熟悉Linux系统的同学应该都理解。

目录划分：

> mkdir /data/gitlab 主要存储利用私有GitLab托管的代码
> 
> mkdir /data/github 主要存放利用GitHub托管的代码
> 
> mkdir /data/\[your company name\] 主要存放公司项目代码或MarkDown文档

安装软件

如下列出的都是提供dmg软件包或者AppStore直接安装，相对简单。

 *  ShadowsocksX-2.6.3
 *  sogou\_mac\_47b
 *  VSCode-darwin-stable
 *  jdk-8u111-macosx-x64
 *  WebStorm-2016.3.4
 *  ideaIU-2017.2.6
 *  OmniGraffle-7.4
 *  OmniPlan-3.7.2
 *  SourceTree\_2.2.4
 *  googlechrome
 *  Evernote
 *  Beyond Compare
 *  Docker.dmg
 *  DockerToolbox.pkg
 *  goland-2018.1.dmg
 *  HipChat-4.30.1-754.dmg
 *  licecap125.dmg
 *  sketch-49.3-51167.zip
 *  SketchBook\_v8.5.1\_mac.dmg
 *  Shimo\_4.1.5.1\_8837.zip
 *  Sublime Text Build 3103.dmg
 *  Tunnelblick\_3.7.4b\_build\_4921.dmg
 *  Office for Mac 2016
 *  ScreenFlow-5.0
 *  Adobe-CC-2018-all

环境变量

开发类的一些软件需要配置环境变量，以便更好地控制与切换版本。

安装oh my zsh

``````````
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
``````````

安装完成，默认主题是`robbyrussell`，你可以通过修改`~/.oh-my-zsh/themes/robbyrussell.zsh-theme`定制主题显示信息。

``````````
local ret_status="%(?:%{$fg_bold[green]% }➜ :%{$fg_bold[red]% }➜ %s)%{$fg_bold[red]% }[%{$fg_bold[blue]% }xujiang%{$fg_bold[yellow]% }@%{$fg_bold[cyan]% }MacBook-Pro"PROMPT='${ret_status}%{$fg_bold[green]% }%p %{$fg[green]% }%c %{$fg_bold[blue]% }$(git_prompt_info)%{$fg_bold[blue]% }%{$fg_bold[red]% }]%{$fg_bold[cyan]% }$%{$reset_color% }% 'ZSH_THEME_GIT_PROMPT_PREFIX="git:(%{$fg[red]% }"ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color% }"ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]% }) %{$fg[yellow]% }✗%{$reset_color% }"ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]% })"
``````````

编辑`~/.zshrc`增加一些自定义配置：

``````````
alias cls='clear'alias ll='ls -l'alias la='ls -a'alias vi='vim'alias javac="javac -J-Dfile.encoding=utf8"alias grep="grep --color=auto"alias -s html=mate # 在命令行直接输入后缀为 html 的文件名，会在 TextMate 中打开alias -s rb=mate # 在命令行直接输入 ruby 文件，会在 TextMate 中打开alias -s py=vi # 在命令行直接输入 python 文件，会用 vim 中打开，以下类似alias -s js=vialias -s c=vialias -s java=vialias -s txt=vialias -s gz='tar -xzvf'alias -s tgz='tar -xzvf'alias -s zip='unzip'alias -s bz2='tar -xjvf'
``````````

插件安装：

可以在`~/.oh-my-zsh/plugins`目录下看到相关插件,默认提供了100多种插件。

启用插件配置`~/.zshrc`文件中找到plugins:

``````````
plugins=(git textmate ruby autojump osx mvn gradle)
``````````

例如 git：当你处于一个 git 受控的目录下时，Shell 会明确显示 「git」和 branch，如上图所示，另外对 git 很多命令进行了简化，例如 gco=’git checkout’、gd=’git diff’、gst=’git status’、g=’git’等等，熟练使用可以大大减少 git 的命令长度，命令内容可以参考~/.oh-my-zsh/plugins/git/git.plugin.zsh

autojump：zsh 和 autojump 的组合形成了 zsh 下最强悍的插件。

``````````
brew install autojump
``````````

安装完成autojump，使用命令 `autojump --help`获取使用方法。

安装Python通过brew

``````````
brew install python
``````````

如上，安装完成之后的Python会自带pip，setuptools等软件，很好的管理Python包。

默认安装的Python是最新稳定的3.x版本。如果需要安装2.x，使用命令`brew install python@2`。

安装完成之后执行如下：

``````````
echo 'export PATH="/usr/local/opt/sqlite/bin:$PATH"' >> ~/.zshrc
``````````

我没��行这一句话，因为我默认使用Python 2.7。

`注意：`如果你使用pyenv管理你的Python版本，那么其实不需要通过brew安装Python，就不用执行此内容。

``````````
brew uninstall python
``````````

如上卸载命令，可以方便的卸载通过brew安装的软件包。

安装Python版本管理工具pyenv

Simple Python Version Management: pyenv

You can also install pyenv using the Homebrew package manager for Mac OS X.

``````````
brew updatebrew install pyenv
``````````

在zsh中启用pyenv需配置如下内容到`~/.zshrc`：

``````````
eval "$(pyenv init -)"
``````````

通过pyenv安装Python 2.7.15版本，通过命令`pyenv install --list`查看可支持安装的Python版本。

``````````
pyenv install 2.7.15
``````````

在安装一个`Python 3.4.0`版本，然后试试切换不同版本是否流畅。

``````````
pyenv install 3.6.5
``````````

查看安装的Python版本列表:

``````````
$ pyenv versions* system (set by /Users/xujiang/.pyenv/version)2.7.153.6.5
``````````

设置2.7.15为全局Python环境：

``````````
pyenv global 2.7.15 # 设置全局的 Python 版本，通过将版本号写入 ~/.pyenv/version 文件的方式。pyenv local 2.7.15 # 设置 Python 本地版本，通过将版本号写入当前目录下的 .python-version 文件的方式。通过这种方式设置的 Python 版本优先级较 global 高。
``````````

会话级别Python环境变量。

``````````
pyenv shell 2.7.3 # 设置面向 shell 的 Python 版本，通过设置当前 shell 的 PYENV_VERSION 环境变量的方式。这个版本的优先级比 local 和 global 都要高。–unset 参数可以用于取消当前 shell 设定的版本。$ pyenv shell --unset$ pyenv rehash # 创建垫片路径（为所有已安装的可执行文件创建 shims，如：~/.pyenv/versions/*/bin/*，因此，每当你增删了 Python 版本或带有可执行文件的包（如 pip）以后，都应该执行一次本命令）
``````````

pyenv 全部���令：

``````````
pyenv commands
``````````

通过pyenv可以很好的解决Python多版本管理问题，并且在各个不同版本间方便的切换，在`VS code`中，我就可以为不同Python项目配置使用不同Python版本。

Virtualenv

前面，我们介绍了基于pyenv设置全局Python环境为`Python 2.7.15`。

现在我们在Python 2.7.15环境，安装`Virtualenv`支持基于此Python版本的多PY项目环境虚拟化。

安装 virtualenv

``````````
pip install virtualenv
``````````

提示升级`pip`：

``````````
pip install --upgrade pip
``````````

使用virtualenv：

``````````
virtualenv env # 创建一个env虚拟Python环境。source env/bin/activate # 激活env虚拟Python环境。pip install pandas # 在激活的env环境下安装pandas包。
``````````

quickstart pandas测试：

``````````
>>> import pandas as pd>>> import numpy as np>>> s = pd.Series([1,3,5,np.nan,6,8])>>> s0 1.01 3.02 5.03 NaN4 6.05 8.0dtype: float64
``````````

如需退���`env`环境，可执行`deactivate`命令。

如果是Python 3.x环境，可以使用官方自带`venv`软件，达到同样的目的。

安装maven

``````````
brew install maven # /usr/local/Cellar/maven/3.5.3
``````````

配置环境变量：

``````````
cat ~/.bash_profile# by xujiang 2018.05.11export M2_HOME=/usr/local/Cellar/maven/3.5.3export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Homeexport PATH=.:$JAVA_HOME/bin:$M2_HOME/bin:$PATH
``````````

下一步��准备写一写如何打造最强工作空间，保障身体健康，更轻松愉快的写代码，得从电脑、屏幕、键盘、鼠标、座椅方面展开。

参考地址：

\[1\] ZSH shell http://macshuo.com/?p=676

\[2\] http://einverne.github.io/post/2017/04/pyenv.html

\[3\] https://docs.python.org/3/library/venv.html
 *  **原文作者：** 易单APP定制
 *  **原文链接：** https://www.toutiao.com/item/6557938227448119822/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。