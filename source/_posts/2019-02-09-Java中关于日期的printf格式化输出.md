---
title:      Java中关于日期的printf格式化输出
date:       2019-02-09
tags:
    - Java
categories: 
	- Java
---



>If you believe in yourself enough and know what you want, you're gonna make it happen.
>如果你足够自信，也知道自己想要什么，就一定会实现心中所想。
																>-----Mariah Carey, "Make It Happen"


printf方法可以很轻易的格式化日期，用两个字母，以%t开头并以下面的其中一个字母结尾。
转换符|说明
---|---|---|-----
a|日期中星期简称
A|日期中星期全称
b|日期中月份的简称
B|日期中月份的全称
C|日期中年份的前两位数
y|日期中年份的后两位数
Y|日期中年份的完整表示
D|“年/月/日”格式
F|“年-月-日”格式
j|一年中的第几天
m|日期中的月份，两位数字表示，不足两位补零
d|日期中的日，两位数字表示，不足两位补零
e|日期中月份的日，不补零
T|“HH:MM:SS” 格式（24小时）
r|“HH:MM:SS PM”格式（12小时）
R|“HH:MM”格式（24小时）

## 测试
~~~java
public class Sinclair_java_20190122
{
	public static void main(String[] args) 
	{
		Date date = new Date();
		String str = String.format(Locale.US,"英文月份简称：%tb",date);
		System.out.println(str);
		System.out.printf("本地月份简称：%tb\n",date);
		str = String.format(Locale.US, "英文月份全称：%tB",date);
		System.out.println(str);
		System.out.printf("本地月份全称：%tB%n",date);
		str = String.format(Locale.US, "英文星期简称：%ta",date);
		System.out.println(str);
		System.out.printf("本地星期的全称：%tA%n",date);
		System.out.printf("本地星期的简称：%ta%n",date);
		System.out.printf("年的前两位数字：%tC%n",date);
		System.out.printf("年的后两位数字：%ty%n",date);
		System.out.printf("年的完整表示：%tY%n",date);
		System.out.printf("一年中的天数：%tj%n",date);
		System.out.printf("%s %tY-%<tm-%<td%n","日期的完整表示：",date);
		System.out.printf("在本月是第几天(不补零)：%te%n",date);
		System.out.printf("在本月是第几天(补零)：%td%n",date);
	}
}
~~~
<!-- more -->
### 测试结果
~~~c++
英文月份简称：Feb
本地月份简称：2月
英文月份全称：February
本地月份全称：二月
英文星期简称：Sat
本地星期的全称：星期六
本地星期的简称：周六
年的前两位数字：20
年的后两位数字：19
年的完整表示：2019
一年中的天数：040
日期的完整表示： 2019-02-09
在本月是第几天(不补零)：9
在本月是第几天(补零)：09
~~~















-----
大家新年好哇！

----
