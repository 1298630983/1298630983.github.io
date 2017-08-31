---
layout: post
title:  "Java简介"
categories: Java
tags: java
author: Jason
excerpt_separator: "###"
---

* 目录
{:toc}

## ！ 数据结构

### 学习时的错误做法：

- 只看不练
- 只听不练
- 钻到细节拔不出来

   |-- 应明确目标
- 心存敬畏

   |-- 不敢动（装卸载软件，系统）
   
   |-- 不敢调试bug
   

---
### Java发展

打孔机-->汇编-->C,C++,Java

### Java核心机制

- Java虚拟机（JVM）

```bash
|-- JVM可以理解成一个以字节码为机器命令的CPU
|-- “一次编译，到处运行”
|-- 不同平台，不同编译器

```

- 垃圾收集机制（Garbage collection）

---

> C,C++为编译型语言，Java为解释型语言

### JDK&JRE

![JDK&JRE](/img/psb.png)

#### JDK目录结构
```
|-- jdk
    |-- bin(binary二进制)：存放编译好的程序
    |-- lib: 库文件
    |-- jre: 运行环境
    |-- src.zip: java源代码

```

#### 配置环境变量

- Path：windows系统执行命令时要找的路径
- Classpath：java在编译和运行时要找的class所在路径


