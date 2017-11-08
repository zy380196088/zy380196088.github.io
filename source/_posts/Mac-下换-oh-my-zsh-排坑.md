---
title: Mac 下换 oh my zsh 排坑
categories: [善其事必先利其器]
date: 2017-04-19 00:18:53
tags: [Mac]
---

Mac下默认的终端使用的是bash,有没有更好的? 当然非zsh莫属咯.

<!--more-->

Linux / Unix 提供了很多种 Shell , 例如 sh , bash , csh等;
可以通过命令行查看:
```
cat /etc/shells
```

# 设置当前用户使用 zsh
```
chsh -s /bin/zsh
```
输入当前用户密码即可;

# 安装 oh my zsh
[oh-my-zsh] : https://github.com/robbyrussell/oh-my-zsh
```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```
或者
```
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
```

# 创建一个zsh的配置文件

注意:如果你已经有一个~/.zshrc文件的话，建议先做备份。
```
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```


# 重启终端

# 修改主题
如果你是个外貌协会的主题控,可以编辑 ~/.zshrc来修改主题:
```
vi ~/.zshrc
```
主题文件在 ~/.oh-my-zsh/themes 目录,选个自己喜欢的,并修改双引号里的主题名字, 例如:ZSH_THEME = "ys"

# 出现的问题(坑)
``source ~/.bash_profile``
但是每次关闭终端重新打开都要source才能重新生效,难道每次打开终端都要重新 source 一下吗! 
我拒绝!
>terminal init的时候并不会执行~/.bash_profile、~/.bashrc等脚本了
>这是因为其默认启动执行脚本变为了～/.zshrc
>解决办法就是修改～/.zshrc文件，其中添加:

```
source ～/.bash_profile、～/.bashrc
```

