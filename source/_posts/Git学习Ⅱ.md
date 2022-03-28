---
title: Git学习Ⅱ
date: 2021-07-22 17:59:31
tags:
  - git学习
categories: Git
---

#### 10.alias命令别名提高操作效率

使用alias重新定西提交

```shell
[alias]
	a = add .
	c = commit
	l = log
	s = status 
```

#### 11.git分支branch存在的意义

Git分支是由指针管理起来的，所以创建、切换、合并、删除分支都非常快，非常适合大型项目的开发。

在分支上做开发，调试好了后再合并到主分支。那么每个人开发模块式都不会影响到别人。

 <!--more-->



#### 12.实例讲解分支branch基本操作

$ git branch xxx 创建分支

$ git checkout xxx 切换分支

$ git branch 查看分支

$ git checkout -b xxx 创建并切换分支

创建新功能要创建并切换到新的分支，不污染主分支



#### 13.分支的合并

$ git master xxx 合并分支

$ git branch -d xxx 删除分支



#### 14.分支管理 --merged 和 --no-merged 及分支强制删除操作

 $ git branch --merged 查看合并分支

$ git branch --no-merged 查看未合并分支

$ git branch -d xxx 删除分支

$ git branch -D xxx 强制删除分支



#### 15.标准的分支操作工作流

![image-20210717105541895](C:\Users\22584\AppData\Roaming\Typora\typora-user-images\image-20210717105541895.png)





#### 16.stash临时储存区实例讲解

当需要跳到其他分支去完成任务时，如果当前文件不需要提交，可以将当前文件放入暂存区

```shell
$ git stash
```



```shell
$ git stash list 查看暂存区文件
```



```shell
$ git stash pop 删除暂存区
```



```shell
$ git stash apply stash@{1} 恢复第一个文件，切换某一个文件存储状态
```

#### 17.git的Tag标签

稳定版

```shell
$ git tag 查看标签
```



```shell
$ git tag v1.0 打标签
```

#### 18.生成zip代码压缩包

```shell
$ git archive master --prefix='lyh' --form=zip > xxx.zip  创建压缩包
```

#### 19.使用系统别名定义git全局指令

```shell
alias gs="git status"alias gc="git commit -m"alias gl="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit  "alias gb="git branch"alias gb="git branch"alias ga="git add"alias go="git checkout"
```

