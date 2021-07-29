## Dos


### 1.什么是批处理？可以做什么？内部命令
  
    1.微软系统自带的原生开发语言，不需要构建任何开发环境就可以执行的脚本。
    2.可以匹配文件。新建文件日志这些
    3.ping cls ipconfig 


### 2.输出Hello World

``` .bat
//关闭命令显示
@echo off 
echo Hello World
pause
```

### 3.批处理运算操作

##### 1.算术运算

    1.* / % + -  (命令模式：cmd输入 "set /a 1+2")

    2.括号改变执行优先级 (文本模式：先要定义变量接收结果，然后用%号包裹输出变量值)

```.cmd
@echo off
set /a a=1+2
echo %a%
pause
```
##### 2.重定向运算

|符号|定义|
|-|-|
|>|结果会覆盖原有内容|
|>>|追加到已有内容后面|

```.cmd
@echo off
set /a tmp=10+2
echo %tmp% > put.txt
set /a tmp_1 = 10*20
echo %tmp_1% >> put.txt
```

##### 3.多命令运算

|符号|定义|备注|
|-|-|-|
|&&|&&之前的命令执行错误就不会执行 && 后面的内容|aaa && net user|
|\|\||第一个执行错误会执行第二个|aaa|| net user |

##### 4.管道符号 A|B 

`将第一个参数的结果作为第二个参数的输入` 

    1.假设a文件夹下有b和c文件夹，1.txt和2.txt文件，查找以txt后缀结尾的文件,并输出到tmp.txt文件中

```.cmd
@echo off
dir | find ".txt" > tmp.txt
pause
```
    2.查找当前主机和其他地址建立网络连接的内容,并输出到tmp.txt文件中。

```.cmd
@echo off
netstat -an | find "ESTABLISHED" > tmp.txt
pause
```


### 4.批处理基本命令

|内置命令|说明|输入|
|-|-|-|
|net user|查看用户|net user|
|netstat -an|查看网络连接|查看对应的TCP udp连接|
|dir|查看目录|dir|
|dir\|find ".txt" |查找文件|dir\|find ".txt"|
|set|单个查看环境变量|set|
|set|变量赋值|set a=123|
|mkdir|建立文件夹或者文件|mkdir file ;mkdir 1.txt|
|/? /help|获取帮助信息|netstat /?或者 netstat /help 详细帮助|

##### 1.批处理文件参数传递

`%1 代表第一个参数，%2代表第二个参数，参数之间用空格隔开`

```.cmd
@echo off
echo %1
echo %2 
pause
@rem 执行这个批处理文件 test.cmd 参数1 参数2
```
        例如有一个test.cmd批处理，跳转到所在目录然后执行test.cmd 参数1 参数2 
    就可以调用这个批处理输出 用户输入的2个参数值
