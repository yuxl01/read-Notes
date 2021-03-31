## Dos


### 1.什么是批处理？可以做什么？内部命令
  
    1.微软系统自带的原生开发语言，不需要构建任何开发环境就可以执行的脚本。
    2.可以匹配文件。新建文件日志这些
    3.ping cls ipconfig


### 2.输出Hello World

``` .bat
@echo off
echo Hello World
pause
```
### 3.批处理运算操作

###### 1.算术运算

    1.* / % + -  (命令模式：cmd输入 "set /a 1+2")

    2.括号改变执行优先级 (文本模式：先要定义变量接收结果，然后用%号包裹输出变量值)

```.cmd
@echo off
set /a a=1+2
echo %a%
pause
```
###### 2.重定向运算

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

###### 3.多命令运算

|符号|定义|备注|
|-|-|-|
|&&|&&之前的命令执行错误就不会执行 && 后面的内容|aaa && net user|
|\|\||第一个执行错误会执行第二个|aaa|| net user |
