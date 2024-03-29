---
title: UTF-8, ASCII, Unicode 的介绍与区分
layout: post
categories: 编程
tags: utf-8 ascii unicode
excerpt: 关于 UTF-8，ASCII，Unicode 的介绍与区分
---
# 背景

人类能通过肉眼识别文字和字符，并能通过知识了解他们的含义，但是计算机内部不论存储还是控制，都是通过二进制码实现，因为二进制的 `0`, `1` 刚好对应基础电路中的**开**和**关**，然后组合进行复杂的系统控制；

将人类识别的字符转换成计算机识别的二进制数据的过程，叫做**编码**，顾名思义，编程二进制数字码，如 `0101101100011001`这样的；相反，就叫做**解码**，把二进制码解释为字符；

# ASCII

首先ASCII全称(American Standard Code for Information Interchange，美国信息交换标准代码)，是一个**字符集**，顾名思义，很多字符的集合；

像前面提到的，人类与计算机语言不通，一个识别字符，一个识别二进制，所以**ASCII**就充当了这样一个**翻译官**，其内容是编码与字符的映射，即一个字符只对应一个固定的编码，例如字符 `A` 的编码为 `65`，字符 `a` 的编码为 `122`；当然这个编码是**十进制**的，计算机内部把十进制转换成二进制就能供底层使用了；

另外需要知道的是，一字节(1 Byte)等于八比特位(8 bit)，8 bit就是这样的：`01010101`，八位二进制的所有不同表示一共 **2^8 = 256** 个，而且一般都是从 **0** 开始数，所以表示的十进制数的范围就是 **0 - 255**，这也是ASCII编码映射字符数的范围，包含大小写字母和一些其他常用的符号；

# Unicode

看完上面肯定就会疑惑ASCII总共才表示256个字符，怎么处理当今世界巨大的信息量的，由于这个字符集最初是老外发明的，表示所有字母和一些字符对他们当时来说可能很足够了，但是先进计算机遍布全球大部分地方，汉语、韩语、日语、阿拉伯语等语言数不过来，所以ASCII明显不够用了；虽然中国之前也制定了是和中文的编码字符集叫 **GB2313** 等系列；

因此，便顺应时代需要产生了一种更庞大的字符集叫 **Unicode**，有时也叫**万国码**，顾名思义，几乎表示了世界上所有语言的字符，可以理解为 `Unique code`，独一无二的的编码；

目前Unicode的编码范围达到了**21位**，即 `0x0000 - 0x10ffff` 的范围，二进制为 `1 0000 1111 1111 1111 1111`，刚好21位；十进制表示为 `1114111`，就是一百万多个字符，已经相当多了；

如果要使用UNIcode，以在 **HTML** 中为例，假如知道一个字符的Unicode码是 `0x0394`,那么就在标签中添加代码：
```html
&#x0394;
```

放在标签中就是：
```html
<h5>这个Unicode码对应的字符是：&#x0394;</h5>
```

结果是这样：

> <h5>这个Unicode码对应的字符是：&#x0394;</h5>

其实那个Unicode编码就是对应的大小希腊字母德尔塔，数学或物理中经常用到的字符；

也可以用 JavaScript 来遍历一部分Unicode与字符的对应关系：
```js
for (i = 0x0000; i <= 0x00ff; i++) {
    document.write(i + ': &#x' + i + ';<br>');
}
```

页面就会出现前256个字符及其Unicode码；

# UTF-8, UTF-16, UTF-32

首先UTF全称(Unicode Transformation Format)，所以它是一种针对前面提到的Unicode的**编码格式**，常见的格式就是 **UTF-8**，还有 `UTF-16`, `UTF-32`；

UTF-8 其中的 `8` 表示的是 `8 bit`，即Unicode中每8位表示一个字符，UTF-16 和 UTF-32 类似，因为Unicode最多才21位，32位大于21位，所以 UTF-32 的格式就可以表示所有字符对应的Unicode码了，但是呢，32位也就是**4字节**，让每个字符都占用4字节太费空间了，所以出现了**UTF-8**和**UTF-16**;

UTF-8 定义 `0 - 7 bit` 的 Unicode 用一字节表示，这里就与**ASCII**一样了，`8 - 11 bit` 用两字节表示，`12 - 16 bit` 用三字节表示，`17 - 21 bit` 用四字节表示; 

UTF-8 编码规则如下：
<style>
table th, table td {
    border: 2px solid black;
}
</style>
|     Unicode     |   bit  |   UTF-8   |   byte   |
|:---------------:|:------:|:----------|:--------:|
| 0x0000 - 0x007f | 0 - 7  | 0XXX XXXX |     1    |
| 0x0080 - 0x07ff | 8 - 11 | 110X XXXX 10XX XXXX |  2 |
| 0x0800 - 0xffff | 12 - 16 | 1110 XXXX 10XX XXXX 10XX XXXX |  3  |
|0x1 0000 - 0x1f ffff|17 - 21|1111 0XXX 10XX XXXX 10XX XXXX 10XX XXXX|4|

规律是：
- 每个字节中不足8位的，高位(左边)先用0补上，比如 `0XXXX XXXX`；
- 超过两字节表示的UTF-8，第一个字节高位添加两个 `1` 和一个 `0`，后面的字节高位添加 `10`；
- 三、四字节同理，几个字节高位就添几个 `1` 再加上一个 `0`，其余字节高位添 `10`；

可以看出 UTF-8 这种针对不同位数使用不同字节数编码的方式有效的利用了空间，避免了一些浪费，当然，事物都有利弊，空间降下去，时间也就升上去了；