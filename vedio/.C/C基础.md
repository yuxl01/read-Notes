# 编译执行过程

### 一、C语言源码文件编译

###### .c预处理(gcc -E a.c)
  
    1、展开头文件
    2、删除注释
    3、条件编译
    4、宏定义展开
    
###### 编译
    1、检查语法
    2、将高级语言转为汇编
   
###### 汇编
    1、将汇编语言转为机器语言二进制

###### 链接 (gcc a.o -o a.exe)
    1、链接不同操作系统库文件,可执行文件
    
    
 ### 二、程序执行
     内存四区： 1、代码区 2、数据区 3、栈区 4、堆区
     
    1、从硬盘加载内存中
    2、内存与cpu交互并进行处理
    
### 三、CPU与寄存器

      CPU离的最近的是寄存器，然后就是cpu缓存,内存
      
    1、寄存器是cpu内部最基础的存储单元。
    2、cpu通过总线(地址,控制、数据)和外部设备交互,总线宽度8位
       寄存器8位，那么cpu就叫8位cpu。
    3、总线16位.寄存器32位，cpu为准32为cpu
    
###### `寄存器`

|8位|16位|32位|64位|
|-|-|-|-|
|A|AX|EAX|RAX|
|B|BX|EBX|RBX|
|C|CX|ECX|RCX|
|D|DX|EDX|RDX|
    
 ### 四、C语言嵌套汇编代码
 ```.c
 #include<stdio.h>
#include<stdlib.h>
int main(void)
{
	int a, b, c;
	a = 10;
	b = 20;
	c = a + b;
	printf("%d\n", c);

	__asm {
		mov a, 3;
		mov b, 4;
		mov eax, a;
		add eax, b;
		mov c, eax;
	}
	printf("%d\n", c);
	return 0;
}
 ```
