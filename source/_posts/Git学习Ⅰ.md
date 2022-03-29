---
title: Git学习Ⅰ
date: 2021-07-21 10:35:15
tags:
  - Git
---

# Git学习Ⅰ

#### 1.配置作者信息

```shell
$ mkdir git  创建文件夹
```

```shell
$ ll 显示 目录列表
```

```shell
$ pwd 看当前在那个文件夹 
```

```shell
$ cd 会文件夹目录
```

 <!--more-->

```shell
$ cd ..后退
```

```shell
$ ls -a 看见隐藏文件
```

```shell
$ git config --global user.email "xxx@xxx.com"
```

```shell
$ git config --global user.name "xxx"
```

```shell
$ rm -rm 删除文件夹
```

```shell
$ git clone https://github.com/xxx/xxx.git 克隆存在的项目
```

#### 2.流水线操作

![image-20210716202644090](C:\Users\22584\AppData\Roaming\Typora\typora-user-images\image-20210716202644090.png)

#### 3.使用命令完成流水操作

```shell
$ touch xxx.txt 创建文件
```

```shell
$ git add xxx.txt 将文件放入小推车（暂存）
```

```shell
$ git status 查看库的文件状态
```

```shell
$ git add .传入所有文件
```

```shell
$ git commit -m 'xxx' 向git库传入文件并说明
```

#### 4.gitignore详解控制版本库文件管理

在gitignore中控制上传文件（.gitignore中是忽略文件）

```shell
$ git .gitignore
```

![image-20210716210833953](C:\Users\22584\AppData\Roaming\Typora\typora-user-images\image-20210716210833953.png)

#### 5.从版本库中删除资源

```shell
$ git rm file 删除库中及本第文件
```

```shell
$ git rm --cached file 仅仅删除本地文件
```

#### 6.版本库中修改资料名称

```shell
$ git mv xxx.txt changeName.txt 修改名字
```

```shell
$ git commit -m 'add file' 上传暂存区的文件
```

```shell
$ git mv indexController.txt IndexController.txt 已存在文件的名字修改
```

在文件夹中修改名称也需要重新提交

![image-20210716213618526](C:\Users\22584\AppData\Roaming\Typora\typora-user-images\image-20210716213618526.png)

#### 07.使用log日志查看历史记录

```shell
$ git log -p 显示文件变动
```

```shell
$ git log --name-only 只显示名字
```

```shell
$ git log -p -1  显示行数
```



#### 08.使用amend修改最新一次提交事件

```shell
$ git commit --amend
```

进入vim界面修改日志信息

i：insert 插入信息

esc：退出插入

:wq：保存编辑并推出Vim

#### 9.管理暂存区中的文件



```shell
$ git rm --cached xxx.txt  文件没有提交 从暂存区撤回
```

```shell
$ git reset xxx.txt  文件已经提交 从暂存区撤销
```

```shell
$ git checkout -- xxx.txt 返回上一次修改内容 回到最初状态
```

```shell
$ cat xxx.txt  查看文件内容
```

