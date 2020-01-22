---
title:  黄金连分数--BigDecimal的使用    
date: 2019-11-22
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: true
# img: /source/images/xxx.jpg
tags:
    - BigDecimal
    - 蓝桥杯
categories:
	- 蓝桥杯
---

一道2013蓝桥杯省赛试题

**黄金连分数**

- 黄金分割数是0.61803是一个无理数，...
- 写出精确到小数点后100位精度的黄金分割数




$$\frac{1}{1+\frac{1}{1+\frac{1}{1+\frac{1}{1+\frac{1}{1+\frac{1}{1+}}}}}}$$

当然黄金分割数是$\frac{\sqrt5-1}{2}$,可以直接算，但是Windows下的计算器的求解只能到小数点之后32位

![windows下计算器计算结果](https://img-blog.csdnimg.cn/20191122163245826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NpbmNsYWlyV2FuZw==,size_16,color_FFFFFF,t_70)

### 思路
当然这是一道填空题，只需要结果，假如你懂一点点java，又刚好知道BigDecimal这个类，那你就可以打开桌面的Eclipse轻松求解这个题目。

```java
import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.Scanner;

public class BigDecimalLearning {
	public static void main(String[] args) {
		BigDecimal res = new BigDecimal(1); // 没有 new BigDecimal(BigDecimal.ONE); 这种构造方法
		for(int i=0;i<100;i++) {
			res = res.add(BigDecimal.ONE);
			res = BigDecimal.ONE.divide(res,105,BigDecimal.ROUND_HALF_UP);
		}
		String str = res.toString().substring(2, 102);
		System.out.println(str);
//		System.out.println(str.length());
	}
}
```
结果：

```java
6180339887498948482045868343656381177203096998094118264936291294152016354093729191617385189497190563
```
<br>

## 简单介绍一下BigDecimal
Java在java.math包中提供了BigDecimal类，用来对超过16位有效位的数进行精确的运算。双精度浮点型变量double可以处理16位有效数。在实际应用中，需要对更大或者更小的数进行运算和处理。float和double只能用来做科学计算或者是工程计算，在商业计算中要用java.math.BigDecimal。
常用方法如下：

```java
BigDecimal b1 = new BigDecimal("20");
BigDecimal b2 = new BigDecimal("30");
b1.add(b2)          //加法，求两个BigDecimal类型数据的和。
b1.subtract(b2);     //减法，求两个BigDecimal类型数据的差。
b1.multiply(b2);     //乘法，求两个BigDecimal类型数据的积。
b1.remainder(b2);    //求余数，求b1除以b2的余数。
b1.max(b2);        //最大数，求两个BigDecimal类型数据的最大值
b1.min(b2);       //最小数，求两个BigDecimal类型数据的最小值。
bi.abs();      //绝对值，求BigDecimal类型数据的绝对值。
b1.negate();    //相反数，求BigDecimal类型数据的相反数。
```
