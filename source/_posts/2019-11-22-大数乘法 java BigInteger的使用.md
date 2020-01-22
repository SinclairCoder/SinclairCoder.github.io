---
title: 大数乘法 java BigInteger的使用     
date: 2019-11-22
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: false
# img: /source/images/xxx.jpg
tags:
    - BigInteger
    - 蓝桥杯
categories:
	- 蓝桥杯
---


找个大数乘法的题目练习一下 BigInterger的使用

>当两个比较大的整数相乘时，可能会出现数据溢出的情形。为避免溢出，可以采用字符串的方法来实现两个大数之间的乘法。具体来说，首先以字符串的形式输入两个整数，每个整数的长度不会超过8位，然后把它们相乘的结果存储在另一个字符串当中（长度不会超过16位），最后把这个字符串打印出来。例如，假设用户输入为：62773417和12345678，则输出结果为：774980393241726.

输入：
>62773417 12345678

输出：
>774980393241726

代码实现

```java
import java.math.BigInteger;
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner input = new Scanner(System.in);
		String str1 = input.next(),str2 = input.next();
		BigInteger num1 = new BigInteger(str1);
		BigInteger num2 = new BigInteger(str2);
		BigInteger res = num1.multiply(num2);
		System.out.println(res.toString());
		input.close();
	}

}

```

比C/C++实现起来爽很多。
一般来说BigInteger是使用String类型进行构造，传入两个个数字字符串，然后进行加减乘除。
java中并没有重载运算符这种操作，只能另外实现封装成新的函数。


一些常用API请参考[java基础-BigInteger的使用](https://blog.csdn.net/suyebiubiu/article/details/78511556)


