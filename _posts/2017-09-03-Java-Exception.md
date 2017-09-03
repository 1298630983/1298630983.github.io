---
layout: post
title:  "Java异常处理"
categories: Java
tags: java
author: Jason
excerpt_separator: ">"
---

* 目录
{:toc}

### 异常概念

> Java运行期间出现的错误 

- Java程序如果在运行期间出现异常事件，可以生成一个异常类对象，该异常对象封装了异常事件的信息并将被提交给java运行时系统，这一过程称为抛出（throw）异常。
- java运行时系统接收到异常时，会处理这段代码并把当前异常对象交给其处理，这一过程称为捕获（catch）异常。

### 异常分类

> Error由java虚拟机生成并抛出，如死循环，内存溢出。RuntimeException一般可不做处理，如被0除，数组下标越界。

- 常见Exception(in java.lang)

  - ClassNotFoundException
  - IOException
  - InterruptException
  - RumtimeException
  
    - ArithmeticException 
    - NullPointerException
    - IndexOutOfBoundsException


### 五个关键字

> **throw, throws, try, catch, finally**

```bash
public void someMethod() throws Exception {
    if(. . .) {
        throw new Exception("错误原因");
    }
}

try {
    someMethod();
}catch(Exception1 e) {
    //异常处理代码
    e.printStackTrace();
    e.getMessage();
}catch(Exception2 e) {
    //异常处理代码
}catch(Exception3 e) {
    //异常处理代码
}finally {
    . . .
}
```
> finally作为异常处理的统一出口，始终执行。

### 自定义异常

- 通过继承java.lang.Exception类声明自己的异常类
- 在方法声明时，用throws声明该方法可能抛出的异常。
- 在方法合适的位置，生成自定义异常实例，throw抛出。

> 注意：重写方法需要抛出与原方法所抛出异常类型一致或不抛出异常。