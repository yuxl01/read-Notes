﻿# Git简单操作及使用
#### 一、Git是什么？
######    Git是目前世界上最先进的分布式版本控制系统（没有之一，高端大气上档次不接受反驳）。

#### 一、Git和GitHub的区别
  1、git是一个版本管理工具,还有SVN,TFS ,VSS都是一个版本管理工具。</br>
  2、GitHub是一个网站，码云也是一个网站，供交流学习的。</br>
  
#### 二、基本的操作
``` .c
1、创建目录初始化一个仓库
   mkdir .File(创建.开头的文件夹必须命令行创建，记录下)
   git init 

2、添加一个文件t.txt到文件夹中,并添加提交到Git仓库。

3、git add . 添加所有文件。

4、git add t.txt 添加指定文件。

5、git commit -m '添加提交描述'。

6、git reset --hard HEAD^ 回滚到上一个版本， ^ 代表一个版本 ^^ 代表2个版本，100个的话就慢慢数吧^--^，
   解决方法是 git reset--hard HEAD~100。

7、git reset --hard 6485f2f71af 不想回滚了，还原回去，在回滚没关闭的情况下，关了你就不知道上一次提交的id了，
   鉴于版本操作有点类似链表形式都是操作指针的,所以速度会是炒鸡块。

```

	 
#### 三、