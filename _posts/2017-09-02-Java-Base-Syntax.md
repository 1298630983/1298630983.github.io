---
layout: post
title:  "Java基本语法"
categories: Java
tags: java
author: Jason
excerpt_separator: ">"
---

* 目录
{:toc}

## Java变量

java变量是程序中最基本的存储单元

> 变量其实是内存中的一小块区域，使用变量名来访问这块区域。
  因此，每个变量使用前必须声明，然后必须赋值，才能使用。
  

## 程序执行的过程

![程序执行过程](/img/pcp.png)

## 数据类型

![数据类型](/img/dt.png)

4类：整型，浮点型，字符型，布尔型

8种：byte,short,long,int; float,double; char; boolean

### boolean
> 取值只有true/false

### char
> Java字符采用Unicode编码，每个字符占两个字节，用十六进制表示

### 整型
> Java各整数类型有固定表数范围和字段长度，其不受操作系统影响，以保证可移植性。

Java整型常量的表示形式：
- 十进制：如，12，-214，0
- 八进制：以0开头 如，012
- 十六进制：以0x或0X开头 如，0x12
> A和10在内存中没区别


类型 | 占用存储空间 | 表数范围
:---:|:---:         |:---:
byte | 1byte | -128 ~ 127
short | 2byte | -2^15 ~ 2^15-1
int | 4byte | -2^31 ~ 2^31-1
long | 8byte | -2^63 ~ 2^63-1

> **注意：整型常量默认为int型，声明long型，常量后加“L”。
    如：long L1 = 88888888888888L; //必须加上L否则报错！**

### 浮点型

表示形式：
- 十进制：3.14，314.0，.314
- 科学记数法：3.14e2, 3.14E2, 100E-2

类型 | 占用存储空间 | 表数范围
:---:|:---:         |:---:
float | 4byte | -3.403E38 ~ 3.403E38
double | 8byte | -1.798E308 ~ 1.798E308

> **浮点型常量默认double，声明float，需常量后加“f”。
    如：float f = 12.3f; //必须加f否则报错**

### 数据类型转换原则
1. 容量小的自动向大的转换：byte,short,char->int->long->float->double
2. byte,short,char它们之间自动转int
3. 容量大的向小的转换时，需加上**强制转换符**，但可能会溢出或降低精度。
如：```float f = (float)0.1; //不等于0.1f，前者是8字节转4字节，后者就是4字节```

### 逻辑运算符
> a^b 异或（相同为false）

> a&&b / a||b 短路与/短路或 （第二个操作数不运算） 

> 三目运算符 `x ? y : z` 说明:如果x为true，则结果为y，否则为z

### 方法
> 方法的本质是增强程序的复用性

### 代码练习

> 输出101~200内的质数（质数是只能被1和自身整除的数，偶数一定不是质数）

```bash
public class TestZhiShu {
    public static void main(String[] args) {
        for(i = 101;i < 200;i+=2) {
            boolean f = true;
            for(j = 2;j < i;j++) {
                if(i % j == 0) {
                    f = false;
                    break; //退出当前for循环
                }
            }
            if(!f) {
                continue; //跳过当前循环
            }
            System.out.println(i);
        }
    }
}
```
> 已知Fibonacci数列：1，1，2，3，5 ... 使用递归和非递归方法求第40个数的值。（递归：在方法内部，对自身进行调用）

```bash
public class Fab {
    public static void main(String[] args) {
        System.out.println(fd(40));
        System.out.println(f(40));
    }
    //使用递归方法
    public static long fd(int index) {
        if(index < 0) {
            System.out.println("invalid parameter!");
            return -1;
        }
        if(index == 1 || index == 2) {
            return 1;
        }else {
            return f(index-1) + f(index-2);
        }
        
    }
    //非递归方法
    public static long f(int index) {
        if(index == 1 || index == 2) {
            return 1;
        }
        long f1 = 1L;
        long f2 = 1L;
        long f = 0;
        
        for(int i = 0;i < index-2;i++) {
            f = f1 + f2;
            f1 = f2;
            f2 = f;
        }
        return f;
    }
}
```




