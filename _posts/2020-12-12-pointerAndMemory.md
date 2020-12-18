---
layout: post
title: "Linux C语言指针与内存"
date: 2020-12-12
---


```bash
vi sample.c
```
以一个简单的数值交换为例，引入`内存`概念。

```c
#include <stdio.h>

void change(int *a,int *b){
	int *temp = a;
	a = b;
	b = temp;
}

int main(void){
	int a,b;
	a=5;
	b=3;
	change(&a,&b);
	printf("a = %d ,b = %d\n",a,b);
	return 0;
}
```
编译
```c
#安装gdb调试工具
sudo apt-get install gdb

#编译
gcc -g sample.c -o sample.out

#运行gdb
gdb sample.out
```
## GDB调试工具常用命令

```bash

```

·
`start`	：进入调试
`break`	：断点
`l`ist 		：显示代码
`回车键`			:重复上一条命令
`p`rint  ` <变量>` ：打印变量
`n`ext		：下一行
`s`tep		：	进入函数？
`bt`			：堆栈状态
`f`	<n>		：切换堆栈
`q`uit	：退出


## 内存结构图

> 其中高位内存是给系统内核用的，程序员只用到其下部分。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201212220325902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTMzNDY3,size_16,color_FFFFFF,t_70)
#### 后续知识概要

`绿色部分`可自由分配`malloc`  

编译后的代码在`代码段`  

变量的内存顺序：`按顺序`  

函数的内存顺序 ：`按逆序`  

静态变量，全局变量 都在`数据段`  

数组：`指针`与`数组`的内存本质  

指针运算：`p++`,步长为数据类型的长度。`p[n]`  

字符数组/指针字符串：
```bash
(gdb) x/6cb 0x7fffffffde03
#连续打印6个字节的字符
```

```c
...
char str[] = "hello";
char *str2 = "world";
char str3[10];
scanf("%s",str);

//若此时输入为aaaaaaaaaaaaaaaaaa，
//超过6个字符的部分进入了str3
```


