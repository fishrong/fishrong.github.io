---
title: Git与GitHub.md
date: 2018-11-17 14:40:11
tags:
  - git
---
# 推送到远程库
* `git remote -v`:查看地址别名  
* 添加别名：
```
git remote add origin git@github.com:fishrong/TestGitInMac.git
```
即用`origin`代替地址。
* 推送
```
git push origin master
```
将`master`分支推送到远程仓库。
# 分支
## 查看当前分支
```
git status
```

## 创建分支
```
git branch hot_fix
```
<!--more-->
## 查看分支（所有）
```
git branch -v
```

## 切换分支
```
git checkout [分支名]
```

## 合并分支
* 切换到被合并分支
* 执行以下命令(将`hot_fix`合并到`master`分支)
```
git merge hot_fix
```

## 向远程库推送
```
git push origin hot_fix
```
将`hot_fix`分支推送到远程库,如果远程库没有这个分支将会新建一个分支

# 从远程库克隆
```
git clone [远程库地址]
```
效果：
* 完整复制远程库文件
* 创建origin别名
* 初始化本地库

# 远程库的抓取
两种方式：
* 1、先`fetch`再`merge`  
* 2、直接使用`git pull origin master`。
* 抓取远程库的`master`分支
```
git fetch origin master
```
此时并不会改变本地库的文件  
查看抓取下来的文件，先切换到远程分支：
```
git checkout origin/master
```
* 合并到本地
```
git merge origin/master
```
