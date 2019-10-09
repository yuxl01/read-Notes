# Git简单操作及使用
#### 一、Git是什么？
######    Git是目前世界上最先进的分布式版本控制系统（没有之一，高端大气上档次不接受反驳）。

#### 一、Git和GitHub的区别
  1、git是一个版本管理工具,还有SVN,TFS ,VSS都是一个版本管理工具。</br>
  2、GitHub是一个网站，码云也是一个网站，供交流学习的。</br>
  
#### 二、基本的操作
1、创建目录初始化一个仓库 </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mkdir .File</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;git init </br>
2、添加一个文件t.txt到文件夹中,并添加提交到Git仓库</br>
3、git add . 添加所有文件</br>
4、git add t.txt 添加指定文件</br>
5、git commit -m '添加提交描述'</br>
6、git reset --hard HEAD^ 回滚到上一个版本， ^ 代表一个版本 ^^ 代表2个版本，100个的话就慢慢数吧0.0，解决方法是 git reset--hard HEAD~100</br>
7、git reset --hard 6485f2f71af 不想回滚了，还原回去，在回滚没关闭的情况下，关了你就不知道上一次提交的id了，因为版本操作都是操作指针的所以速度会是炒鸡块</br>
	 
#### 三、
