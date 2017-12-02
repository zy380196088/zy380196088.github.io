---
title: Git常用命令
date: 2016-04-16 01:24:36
tags: [git]
---

## 用户信息
<pre><code>git config --global user.name “youname”
git config --global user.email “123456@example.com”
</code></pre>
<!--more-->
## Git基础
### 查看配置信息
<pre><code>git config --list
git config -l
</code></pre>

### 初始化新仓库
<pre><code>git init</code></pre>
初始化后，在当前目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。

### 跟踪文件
<pre><code>git add</code></pre>

### 跟踪全部文件
<pre><code>giat add .</code></pre>

### 提脚更新
<pre><code>git commit -m "跟新描述"</code></pre>

### 添加原程仓库
<pre><code>git remote add [shortname] [url]</code></pre>
例如:
<pre><code>git remote add origin git@github.com:yourname/yourRepo.git</code></pre>

### 上传推送到github
<pre><code>git push [remote-name] [branch-name]
git push origin master</code></pre>

如果要把本地的 master 分支推送到origin服务器上(再次说明下，克隆操作会自动使用默认的master 和origin 名字)

### 从远程仓库克隆
<pre><code>git clone git://github.com/youname/project.git
git clone git@github.com:youname/project.git
git clone https://github.com/youname/project.git
</code></pre>

### 从原程仓库抓取数据
<pre><code>git fetch [remote-name]</code></pre>

### 删除原程仓库
<pre><code>git remote rm otigin</code></pre>

### 检查当前文件状态
<pre><code>git status</code></pre>

### 创建.gitginore文件
<pre><code>touch .gitignore</code></pre>

### 查看修改之后 没有暂存起来的内容
<pre><code>git diff</code></pre>

### 移除文件
<pre><code>git rm 文件名.后缀</code></pre>

### 重命名文件
<pre><code>git mv oldname.后缀 newname.后缀</code></pre>

### 查看提交历史
<pre><code>git log</code></pre>

### 取消暂存
<pre><code>git reset 文件名</code></pre>

## Git分支

### 创建名为name的分支
<pre><code>git branch name</code></pre>

### 切换到name分支（默认master分支）
<pre><code>git checkout name</code></pre>

上面两条命令可缩写为：
<pre><code>git checkout -b name</code></pre>

### 删除name分支（如果该分支还未被合并则会提示错误，因为这样会丢失数据）
<pre><code>git branch -d name</code></pre>

强制删除：<pre><code>git branch -D name</code></pre>

### 合并分支（先切换到master分支）
<pre><code>git checkout master
git merge name</code></pre>

### 查看当前所有分支
<pre><code>git branch</code></pre>

### 查看各个分支最后一个提交对象的信息
<pre><code>git branch -v</code></pre>

### 查看已经与当前分支合并的分支（已经合并的查出来后可以删掉）
<pre><code>git branch --merge</code></pre>

### 查看未与当前分支合并的分支
<pre><code>git branch --no-merged</code></pre>
