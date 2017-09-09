---
layout: post
title:  "Java线程"
categories: Java
tags: java
author: Jason
excerpt_separator: "-"
---

* 目录
{:toc}

### 线程的概念

> 线程是一个程序里面不同的执行路径。java中由java.lang.Thread类来实现。

- 对比进程，进程是个静态的概念；机器中运行的都是线程，同一时间点一个cpu只支持一个线程。

### 线程创建和启动

1. 实现Runnable接口（推荐）
2. 从Thread继承
3. 线程启动必须调用Thread的start()方法

### 线程常用基本方法

- sleep() : 静态的
  - wait()方法调用时可以访问锁定对象，sleep()时，别的线程也不可以访问锁定对象 
- join() : 合并某个线程
- yield() : 让出cpu，给其他线程执行的机会
- setPriority() : 线程优先级（1-10，默认为5）

### 线程同步

```
synchronized(this) {
    ...
}
```

> 死锁解决方法，当线程执行时，需要锁定某个对象

- 代码示例

> 生产者消费者问题。生产者往篮子里放窝头，消费者拿窝头。篮子最多放6个窝头。

```
public class ProducerConsumer {
    public static void main(String[] args) {
        SyncStack ss = new SyncStack();
        Producer p = new Producer(ss);
        Consumer c = new Consumer(ss);
        new Thread(p).start();
        new Thread(c).start();
    }
}

class WoTou {
    int id;
    WoTou(int id) {
        this.id = id;
    }
	
	public String toString() {
		return "WoTou : " + id;
	}
}

class SyncStack {
    int index = 0;
    WoTou[] arrWT = new WoTou[6];
    
    public synchronized void push(WoTou wt) {
		while(index == arrWT.length) {
			try {
				this.wait();
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
		this.notify();
        arrWT[index] = wt;
        index++;
    }
    
    public synchronized WoTou pop() {
		while(index == 0) {
			try {
				this.wait();
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
		this.notify();
        index--;
        return arrWT[index];
    }
}

class Producer implements Runnable {
    SyncStack ss = null;
    Producer(SyncStack ss) {
        this.ss = ss;
    }
    
    public void run() {
        for(int i = 0;i < 20;i++) {
            WoTou wt = new WoTou(i);
            ss.push(wt);
            System.out.println("生产了:  " + wt);
			try {
				Thread.sleep(1000);
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
        }
    }
}

class Consumer implements Runnable {
    SyncStack ss = null;
    Consumer(SyncStack ss) {
        this.ss = ss;
    }
    
    public void run() {
        for(int i = 0;i < 20;i++) {
            WoTou wt =  ss.pop();
            System.out.println("消费了： " + wt);
			try {
				Thread.sleep(1000);
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
        }
    }
}
```



