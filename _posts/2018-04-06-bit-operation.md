---
title: 位运算符与位运算
layout: post
excerpt: 编程中位运算的介绍与应用
categories: 编程
tags: 位运算 按位运算
---
# 目录 <span id="home">

* [概述](#1)
* [按位与](#2)
* [按位或](#3)
* [按位异或](#4)
	* [简单应用](#4.1)
    	* [交换变量值](#4.1.1)
    	* [简单加密](#4.1.2)
* [按位取反](#5)
* [移位运算](#6)
	* [左移位](#6.1)
	* [右移位](#6.2)
* [复合赋值符](#7)

# 一、概述 <span id="1">
程序中的所有数在计算机内存中都是以二进制的形式储存的。除了常见的**算术运算符**：`+ - * / %`，还有**位运算**：`& | ^ ~ >> <<`，就是直接对整数在内存中的二进制位进行操作。接下来以C语言为例介绍，其它语言大同小异。

#  二、按位与（&）<span id="2">
又叫 `and` 运算，用符号 `&` 表示，计算方式如下：

`1 & 1 = 1` , `0 & 1 = 0` , ` 0 & 0 = 0`

布尔类型中，`1` 表示真，`0` 表示假，所以可以用高中数学那句话来记：**`同真为真，一假为假`**。

举个例子：`3 & 5 = 1` ，运算过程如下：
<pre>
   0 1 1   ---> 3
 & 1 0 1   ---> 5
  --------
   0 0 1   ---> 1
</pre>
 
# 三、按位或 （|）<span id="3">
又叫 `or` 运算，用符号 `|` 表示，运算方式：

`1 | 1 = 1` , `1 | 0 = 1` , `0 | 0 = 0`

记为：**`一真为真，同假为假`**

举例：`3 | 5 = 7` , 运算过程：
<pre>
   0 1 1   ---> 3
 | 1 0 1   ---> 5
 --------
   1 1 1   ---> 7
</pre>

# 四、按位异或（^）<span id="4">

又叫 `xor` 运算，用符号 `^` 表示，注意这里不是数学表达里面的**次方**的意思，运算方式：

`1 ^ 1 = 0` , ` 1 ^ 0 = 1` , `0 ^ 0 = 0`

这种运算也较为特殊，记为：**`同为假，异为真`**
举例：`3 ^ 5 = 6` , 运算过程：
<pre>
   0 1 1    ---> 3
 ^ 1 0 1    ---> 5
 ---------
   1 1 0    ---> 6
</pre>

## 简单应用 <span id="4.1">

### 1、交换变量值 <span id="4.1.1">

异或运算有如下特性：

`a ^ b ^a = b` , `a ^ b ^ b =a`

因此可以用于程序中的变量值交换，C语言中，我们可能经常这样交换变量值：
``` c
#include <stdio.h>
int main()
{
	int a = 3, b = 5;
	int temp; /*定义一个临时变量用于交换方便*/
	temp = a;
	a = b;
	b = temp;
	printf("a = %d, b = %d", a, b); /*输出结果为：a = 5, b = 3 实现了变量值的交换*/
}
```
但是以后我们可以这样实现：
``` c
#include <stdio.h>
int main()
{
	int a = 3, b = 5;
	a ^= b; /*等同于：a = a ^ b 之后会讲*/
	b = a ^ b; /*这时 a ^ b 等于 原来 a 的值*/
	a ^= b; /*a = a ^ b*/
	printf("a = %d, b = %d", a, b);
}
```
>**a, b 为负数时同样适用，但位运算只适用于整型，浮点数不可用**

### 2、简单加密 <span id="4.1.2">

例如，你想传输一条信息给Ta，信息为：`5201314` ，密码为：`1998` ，使用如下代码加密：

	5201314 ^ 1998

结果为 `5200492`，想要查看原信息，使用密码查看：

	5200492 ^ 1998
这样就生成了原数字：`5201314`.

# 五、按位取反（~）<span id="5">
又叫 `not` 运算，即将二进制位中 `0` 和 `1` 全部取反，运算方式为：

`~ 1 = 0` , `~ 0 = 1`

记为：**`0 变 1，1 变 0`**

举例：`~5 = -6`, 运算过程：
<pre>
 ~ 00000000 00000000 00000000 00000101   ---> 5
 ---------------------------------------
   11111111 11111111 11111111 11111010   ---> -6
</pre>
>**注意变量如果定义是无符号短整型 `unsigned short` ， `~5` 将不再是 `-6` ，而是 `65530`**。
>**还有一个规律是正整数取反后结果是原数加一后取相反数，负数一样**。
>**还要注意不要和逻辑运算符 `!` 搞混，`! 1 = 0` , `! 1234 = 0` , `! 0 = 1`**，即只有 `真(1)` 与`假(0)` 两种。

# 六、移位运算 <span id="6">
顾名思义，移位即将数据的二进制数进行向左或向右平移一定的位数，然后得到新的数据，移位分为左移位和右移位。
## 1、左移位（<<）<span id="6.1">
将数据的转换为二进制，所有位向左平移，高位（左端）舍弃，低位（右端）空位补 `0`，格式为：

`需要移位的数据 << 需要移动的位数`

举例：`5 << 2 =20` ，运算过程：

<pre>
  00000000 00000000 00000000 00000101   ---> 5
 -------------------------------------
  00000000 00000000 00000000 00010100   ---> 20
</pre>
>**右移位的数学意义是，原数左移 n 位，相当于原数乘以 2 的 n 次方**

## 2、右移位 <span id="6.2">
将数据的二进制所有位向右移位，低位舍弃，高位补 `0`（负数补 `1`），格式：

`需要移位的数据 >> 需要移动的位数`

举例：`5 >> 2 = 1`，运算过程：
<pre>
  00000000 00000000 00000000 00000101   ---> 5
 -------------------------------------
  00000000 00000000 00000000 00000001   --->  1
</pre>
负数情况：`-6 >> 2 = -2`，运算过程：
<pre>
  11111111 11111111 11111111 11111010    ---> -6
 -------------------------------------
  11111111 11111111 11111111 11111110    ---> -2
</pre>
>**右移位的数学意义是，相当于原数除以 2 的 n 次方**

# 七、复合赋值符 <span id="7">

算术运算中有复合赋值符：`+= -+ *= /= %=`，位运算也有对应的复合赋值：`&= |= ^= >>= <<= `（注意没有 `~=` ），运算一样：

`a &= b` 等价于 `a = a & b`

以此类推

-----------------------------------------------------------------------------
# 返回[顶部](#home)