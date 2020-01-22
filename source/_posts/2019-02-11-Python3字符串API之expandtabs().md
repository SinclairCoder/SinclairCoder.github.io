---
title:      Python3字符串API之expandtabs()
date:       2019-02-11
tags:
    - Python3
categories: 
	- Python3
---


>Challenges make you discover things about yourself that you never really knew. 
挑战能让你发现自己都不曾了解过的一面。		-----Cicely Tyson



## 描述 

```python3
		def expandtabs(self, *args, **kwargs):  # real signature unknown**
        """
        Return a copy where all tab characters are expanded using spaces.
        If tabsize is not given, a tab size of 8 characters is assumed.
        """
        pass
   
```

--------
>返回字符串的一个拷贝，该拷贝中把字符串中的**tab** (即’\t‘)转换为指定的**tabsize**数量的空格，其中若**tabsize**未给出则默认为8.

--------
<!-- more -->
## 语法
>str.expandtabs(tabsize = 8);

## 实例
其实还是例子更直观

~~~python3
 # 代码段
print("第一行为基准")
print("123456789012345678901")
s1 = "123456\t7890\t123"
print(s1)
print(s1.expandtabs(6))
print(s1.expandtabs(7))
print(s1.expandtabs(8))
print(s1.expandtabs(9))
s2 = "\t12\t345678\t123"
print(s2)
print(s2.expandtabs())
~~~

~~~python3
 # 结果显示
第一行为基准
123456789012345678901
123456	7890	123
123456      7890  123
123456 7890   123
123456  7890    123
123456   7890     123
	12	345678	123
        12      345678  123
~~~

## 总结

> &emsp;&emsp;**expandtabs()** 方法是把字符串中的'\t'转换为空格。
 &emsp;&emsp;下面首先来说说'\t'，在Python3中 **'\t'** 是补4的整数倍个空格，假如'\t'在字符串首，则会补**4**个空格，当在串中时要看'\t'前面的字符，距离**4**的整数倍差多少补多少空格。
 &emsp;&emsp;下面再说一下这个 **expandtabs()** 方法就是在 **'\t'** 处补指定长度**tabsize**的空格，可以自定指定，也可以使用默认的补**8**个空格，即看前面的字符串然后将其补到**tabsize**的整数倍，假如'\t'在串首则直接补**tabsize**数量的空格。


















						
						




