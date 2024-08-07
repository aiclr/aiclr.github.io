+++
title = 'Primitive Types'
date = '2024-07-03'
author = 'aiclr'
categories = ['develop']
tags = ['Primitive Types','java']
Summary='基础数据类型'
+++

## Java

- [https://dev.java/learn/language-basics/primitive-types/](https://dev.java/learn/language-basics/primitive-types/)


### 数据类型所在内存

- 2^10=1024
- 1 KB=1024 Byte = 2^10 Byte

|    类型     | 字节数(Byte) | 位数(bit) |                               取值范围                                |
|:---------:|:---------:|:-------:|:-----------------------------------------------------------------:|
|   byte    |     1     |    8    |                           -2^7 ~ 2^7-1                            |
|   short   |     2     |   16    |                          -2^15 ~ 2^15-1                           |
|    int    |     4     |   32    |                          -2^31 ~ 2^31-1                           |
|   long    |     8     |   64    |                          -2^63 ~ 2^63-1                           |
|  boolean  |     1     |    8    |                            true和false                             |
|   char    |     2     |   16    |           unicode编码，前128字节与ASCII兼容字符存储范围在 \u0000~\uFFFF           |
|   float   |     4     |   32    | 3.402823e+38 ~ 1.401298e-45（e+38表示是乘以10的38次方，同样，e-45表示乘以10的负45次方） |
|  double   |     8     |   64    |                   1.797693e+308~ 4.9000000e-324                   |
| reference |    4/8    |  32/64  |           引用型数据，32位系统或开启指针压缩的64位系统占用4字节，64位系统不开指针压缩占8字节           |

### 浮点型(科学计数法) [进制转换参考](../decimal/)

|  浮点型   |  符号位  |   指数    |   尾数   |      内存占用       |
|:------:|:-----:|:-------:|:------:|:---------------:|
| float  | 1 bit | 8 bite  | 23 bit | 32 bit / 4 Byte |
| double | 1 bit | 11 bite | 52 bit | 64 bit / 8 Byte |


### boolean

虽然定义了 boolean 但是 jvm 并没有任何供 boolean 值专用的字节码指令。\
Java 表达式操作的 boolean 在编译后都使用 JVM 中的 int 来代替。 \
4 字节 boolean 数组被编码为 JVM 的 byte 数组,数组中的每个元素占 8 bit = 1 字节。 \
boolean 使用 int 32 bit 原因是，对于 32 位 CPU 来说具有高效存取的特点 \
具体情况要看 JVM 实现是否符合规范，1 字节、4 字节都有可能。实质是**运算效率**和**存储空间**的博弈