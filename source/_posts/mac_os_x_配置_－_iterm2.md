---
title: Mac OS X 配置 － iTerm2
date: 2016-07-13
---


### [iTerm2](http://www.iterm2.com/)

> 	iTerm2 is a terminal emulator for OS X that does amazing things


![](/images/iterm2.png)

<!-- more -->

要实现这样的效果，需要进行一些简单的配置

#### 颜色配置

+ 首先需要从github上clone一个主题包

```bash
git clone https://github.com/altercation/solarized.git
```
   
+ 然后Import选择该主题包

Prefernces > Profiles > Color Presets > Import...
这里选择刚刚clone下来的项目iterm2-colors-solarized目录
然后选择Solarized Dark.itermcolors文件，重新启动iTerm2

+ 其它一些设置

重启后iTerm2并没有明显改变，还需要取消以下勾选
Prefernces > Text > Draw bold text in bright colors

+ 透明效果
	
先从网上找一张透明背景图片，勾选
Prefernces > window >BackBround Image
选择透明图片，之后可以调整Blending的值来改变透明度

#### bash > zsh

一行简单的命令久可以将bash切换为zsh

```bash
chsh -s /bin/zsh
```
可惜zsh不是很好用，需要配合一些插件/模版，如oh-my-zsh

下载包

```bash
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh 
```

替换zshrc

```bash
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

还可以修改oh-my-zsh主题，.oh-my-zsh/themes目录下有各种主题
```
```

至此就大功告成了





