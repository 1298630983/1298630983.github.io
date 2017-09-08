---
layout: post
title:  "Java容器"
categories: Java
tags: java
author: Jason
excerpt_separator: "-"
---

* 目录
{:toc}

### 容器API

> 位于java.util包内

- Set接口 无序不可重复
- List接口 有序可重复

### Collection接口

> 对象作为索引，必须重写equals和hashcode方法

### Iterator接口

> 为了统一访问**遍历**集合类的接口。Iterator遍历中只能用Iterator中的remove方法。

### 增强for循环

```
for(int i = 0;i < arr.length;i++) => for(int i : arr) 
```

> 缺点：不能方便地访问下标值，不能删除集合中的内容（能看懂就行，不建议使用）

### Comparable接口

> 所有可以“排序”的类都实现了java.lang.Comparable接口。他只有一个方法：public int comparaTo(Object obj);

### 如何选择数据结构

> 衡量的标准：读的效率和改的效率

- Array 读快改慢
- Linked 读慢改快
- Hash 两者之间

### Map接口

> 存储键值对（key-value）。实现类HashMap和TreeMap等。键值不能重复。

### 自动打包/解包

> 自动将基本类型 <-> 对象

### 泛型

- 起因：1.4以前类型不明确
- 解决方法：在定义集合的时候，同时定义集合中对象的类型
 
> 记录args数组中每个字符出现的次数

```
import java.util.*;

public class TestArgsWords {
    public static void main(String[] args) {
        Map<String,Integer> m = new HashMap<String,Integer>();
        
        for(int i = 0;i < args.length;i++) {
            if(!m.containsKey(args[i])) {
                m.put(args[i],1);
            }else {
                int freq = m.get(args[i]);
                m.put(args[i],freq + 1);
            }
        }
        System.out.println(m.size() + " distinct words detected: ");
        System.out.println(m);
    }
}
```

## 总结

- 一个图
- 一个类
  - Collections: 封装常用list算法，如shuffle(),sort(),binarySearch()
- 三个知识点
  - 增强for
  - 泛型（Generic）
  - 自动打包/解包
- 六个接口
  - Collection 所有一个个往里装的最上层接口
  - Set
  - List
  - Map
  - Itertor 标准统一地来遍历Collection
  - Comparable 定两个对象谁大谁小