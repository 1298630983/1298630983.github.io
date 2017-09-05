---
layout: post
title:  "Java常用类"
categories: Java
tags: java
author: Jason
excerpt_separator: "-"
---

* 目录
{:toc}

### String类

> String代表**不可变**的字符序列,StringBuffer代表可变的字符序列。

- 代码示例 

> 输出一个字符串中大写英文字母数，小写英文字母数,其他字符数以及"java"出现的次数。

```
public class TestString {
    public static void main(String[] args) {
        String s = "1234dzxcjava,/.;'afsf3rsfjava.''javaddas";
        
        int lCount = 0, uCount = 0, oCount = 0;
		for(int i = 0;i < s.length();i++) {
			char c = s.charAt(i);
			if(Character.isLowerCase(c)) {
				lCount++;
			}else if(Character.isUpperCase(c)) {
				uCount++;
			}else {
				oCount++;
			}
		}
		
		String sToFind = "java";
		int javaCount = 0;
		int index = -1;
		
		while((index = s.indexOf(sToFind)) != -1) {
			s = s.substring(index + sToFind.length());
			javaCount++;
		}
		
       System.out.println(lCount +" "+ uCount +" "+ oCount);
	   System.out.println(javaCount);
    }
}
```

### 基本数据类型包装类

> 包装类（如Integer,Double,Character等）这些类封装了这些基本数据类型的数值，并提供一系列操作。

- 代码示例

> 编写一个方法，返回一个double型数组，数组中的元素通过解析字符串获得。如字符串参数："1,2;3,4,5;6,7,8"
  对应数组为
  
    d[0,0] = 1.0 d[0,1] = 2.0 
    d[1,0] = 3.0 d[1,1] = 4.0 d[1,2] = 5.0
    d[2,0] = 6.0 d[2,1] = 7.0 d[2,2] = 8.0

```
public class ArrayParse {
    public static void main(String[] args) {
        double[][] d;
        String s = "1,2;3,4,5;6,7,8";
        String[] sFirst = s.split(";");
        d = new double[sFirst.length][];
        for(int i = 0;i < sFirst.length;i++) {
            String[] sSecond = sFirst[i].split(",");
            d[i] = new double[sSecond.length];
            
            for(int j = 0;j < sSecond.length;j++) {
                d[i][j] = Double.parseDouble(sSecond[j]);
            }
        }
        
        for(int i = 0;i < d.length;i++) {
            for(int j = 0;j < d[i].length;j++) {
                System.out.print(d[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

### Math类

> 一系列静态方法用于数学计算，方法的参数和返回值通常为double

### File类

> java.io.File类代表系统文件名（路径和文件名）

- 代码示例

> 编写一个程序，在命令行中以树状结构显示文件夹及其子文件（夹）

```
import java.io.*;

public class TestList {
    public static void main(String[] args) {
        File f = new File("d:/java/A");
		System.out.println(f.getName());
        tree(f,1);
    }
	
    public static void tree(File f,int level) {	
		String preStr = "";
			for(int i = 0;i < level;i++) {
			preStr += "   ";
		}
		
        File[] childs = f.listFiles();
        for(int i = 0;i < childs.length;i++) {
            System.out.println(preStr + childs[i].getName());
            if(childs[i].isDirectory()) {
                tree(childs[i], level + 1);
            }
        }
    }
}
```