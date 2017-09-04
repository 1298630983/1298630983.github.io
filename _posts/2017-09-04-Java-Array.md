---
layout: post
title:  "Java数组"
categories: Java
tags: java
author: Jason
excerpt_separator: "-"
---

* 目录
{:toc}

### 数组概念

- 可以看成多种相同类型数据的集合。
- 数组变量属于引用类型，数组也可以看成是对象，数组中的某个元素相当于该对象的成员变量。
- 数组中的元素可以是任何数据类型。

### 数组声明

> 声明数组不能指定长度。如：int i[3];

### 创建数组

> 数组名 = new 数组元素类型（个数） 如：int[] s = new int[4];

### 数组初始化

- 动态初始化 `int[] a = new int[3]; a[0] = 1;a[1] = 2;a[2] = 3;`
- 动态初始化 `int[] a = {1,2,3};`
 
### 代码示例

> 使用args实现四则运算

```bash
public class TestArgs {
    public static void main(String[] args) {
        if(args.length < 3) {
            System.out.println(" Usage : java TestArgs \"num1\" \"opt\" \"num2\" ");
            System.exit(-1);
        }
        try {
			double num1 = Double.parseDouble(args[0]);
			double num2 = Double.parseDouble(args[2]);
			double sum = 0;
			
			if(args[1].equals("+")) {
				sum = num1 + num2;
			}else if(args[1].equals("-")) {
				sum = num1 - num2;
			}else if(args[1].equals("x")) {
				sum = num1 * num2;
			}else if(args[1].equals("/")) {
				sum = num1 / num2;
			}else {
				System.out.println("Error Opt!");
				System.exit(-1);
			}
			System.out.println(sum);
		}catch(NumberFormatException e) {
			System.out.println("please input right number format!");
		}
    }
}
```

> 选择排序/冒泡排序

```bash
public class NumberSort {
    public static void main(String[] args) {
        int[] a = new int[args.length];
        for(int i = 0;i < args.length;i++) {
            a[i] = Integer.parseInt(args[i]);
        }
        
        print(a);
        selectionSort(a);
        print(a);
    }
    
    //选择排序
    public static void selectionSort(int[] a) {
        int temp,k;
        for(int i = 0;i < a.length;i++) {
			k = i;
            for(int j = k+1;j < a.length;j++) {
                if(a[j] < a[k]) {
                    k = j;
                }
            }
			if(k != i) {
				temp = a[i];
                a[i] = a[k];
                a[k] = temp;
			}
        }
    }
    /*
    //冒泡排序
	public static void bubbleSort(int[] a) {
		int temp;
		for(int i = a.length-1;i >= 1;i--) {
			for(int j = 0;j <= i-1;j++) {
				if(a[j] > a[i]) {
					temp = a[i];
					a[i] = a[j];
					a[j] = temp;
				}
			}
		}
	}
    */
    
    public static void print(int[] a) {
        for(int i = 0;i < a.length;i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println();
    }
    
}
```

> 数三去一，500人围成一圈，每数三个去一个，问最后留下的是第几个？

```bash
public class Count3Quit {
    public static void main(String[] args) {
        boolean[] arr = new boolean[500];
        
        for(int i = 0;i < arr.length;i++) {
            arr[i] = true;
        }
        
        int leftCount = arr.length;
        int countNum = 0;
        int index = 0;
        
        while(leftCount > 1) {
            if(arr[index] == true) {
                countNum++;
                if(countNum == 3) {
                    countNum = 0;
                    leftCount--;
                    arr[index] = false;
                }
            }
            index++; 
            if(index == arr.length) {
            index = 0;
        }
        }
        
        for(int i = 0;i < arr.length;i++){
            if(arr[i] == true) {
                System.out.println(i+1);
            }
        }
    }
}
```

### 二维数组

```bash
int[][] a = new int[3][];
a[0] = new int[2];
a[1] = new int[4];
a[2] = new int[3];
```
> 分析内存，二维数组即数组的数组

### 数组复制

> 使用java.lang.System类的静态方法(arraycopy)

- 代码示例

```bash
public class TestArrayCopy {
    public static void main(String[] args) {
        String[] s = {"Mircosoft","Sony","Apple","Oracle","Amazon"};
        String[] sBak = new String[6];
        
        System.arraycopy(s,0,sBak,0,s.length);
        for(int i = 0;i < sBak.length;i++) {
            System.out.println(sBak[i]);
        }
        
        int[][] a = { {1,2},{3,4,5},{6} };
        int[][] aBak = new int[3][];
        
        System.arraycopy(a,0,aBak,0,a.length);
        for(int i = 0;i < a.length;i++) {
            for(int j = 0;j < a[i].length;j++) {
                System.out.println(a[i][j]);
            }
        }
    }
}
```




