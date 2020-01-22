---
title:   LeetCodeWeeklyContest-155   
date: 2019-09-22
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: false
# img: /source/images/xxx.jpg
tags:
    - leetcode
categories:
	- leetcode
---

>操千曲而晓声,观千剑而后识器           ---- 《文心雕龙》

力扣的周赛自闭了，好久没写了，丢掉的改捡了。
## [题目传送](https://leetcode-cn.com/contest/weekly-contest-155/)
**[个人博客同步更新](https://sinclaircoder.top/2019/09/22/20190922-LeetCodeWeeklyContest-155/)**
## Problem1 最小绝对差  
- 难度：Easy
### 题目
>给你个整数数组 arr，其中每个元素都 不相同。
请你找到所有具有最小绝对差的元素对，并且按升序的顺序返回。

### 思路
- 遍历，边找边push，只需一遍

~~貌似代码写复杂了~~

### 题解
```cpp
#include<bits/stdc++.h>
using namespace std;
/*
	LeetCode WeeklyContest-155
*/
vector<vector<int> > minimumAbsDifference(vector<int>& arr) {
   sort(arr.begin(),arr.end());
   int n=arr.size();
   vector<vector<int> > result; 
   int min = arr[1]-arr[0];
   vector<int> tmp;
   for(int i=0;i<n-1;i++){
   		if(arr[i+1]-arr[i] < min){
   			min = arr[i+1]-arr[i];
   			result.clear();
   			tmp.push_back(arr[i]);
			tmp.push_back(arr[i+1]);
			result.push_back(tmp);
			tmp.clear();
		}
		else if(arr[i+1]-arr[i] == min){
			tmp.push_back(arr[i]);
			tmp.push_back(arr[i+1]);
			result.push_back(tmp);
			tmp.clear();
		}
		else ;
   } 
     return result;  
}

// 测试下
int main()
{
	vector<int> v;
	v.push_back(-20);		
	v.push_back(11);		
	v.push_back(26);	
	v.push_back(27);
	v.push_back(40);	
	vector<vector<int> > result = minimumAbsDifference(v);
	vector<vector<int> >::iterator rIt;
	vector<int>::iterator it;
	for(rIt = result.begin();rIt!=result.end();rIt++){
		for(it = (*rIt).begin();it!=(*rIt).end();it++){
			cout << *it << " ";
		}
		cout << endl;
	}
	
	
	return 0;
 } 
```


## Problem 2 丑数 III   
### 题目
> 请你帮忙设计一个程序，用来找出第 n 个丑数。
 >丑数是可以被 a 或 b 或 c 整除的 正整数。
 >1 <= n, a, b, c <= 10^9
1 <= a * b * c <= 10^18
本题结果在 [1, 2 * 10^9] 的范围内

- 难度：Medium
- 
### 思路
- 无脑暴力$1到\min(n*a,n*b,n*c)$ 超时。
- 无脑开个数组，把 $i*a、i*b、i*c$ 其中$i=1...n$,然后写个sort(),直接取第n个数，内存爆掉。

以上都是不可取的
**计算1~m之间有多少丑数，设为cnt，然后从n开始折半查找，判断cnt与n的关系，最后直接定位到第n个丑数。
另外，如何判断1~m之间有多少丑数？公式如下：
$$cnt = m/a + m/b + m/c  - m/lcm(a,b)- m/lcm(b,c) - m/lcm(a,c) + m/lcm(lcm(a,b),c)$$
其中$lcm(a,b)$是a、b的最小公倍数。**


### 题解
```cpp
/*
	LeetCode WeeklyContest-155
*/

#include<bits/stdc++.h> 
using namespace std;
typedef long long ll;
// 避免超范围 ，不妨都写成ll
ll gcd(int x, int y){
	if(x>y) swap(x,y);
	int r = y%x;
	if(r==1) return 1;
	while(r){
		y = x;
		x = r;
		r = y%x;
	}
	return x;
}
ll lcm(int x, int y){
	return x/gcd(x,y)*y;
}
bool countLessN(ll m,int a,int b,int c,int n){
	int cnt = m/a + m/b + m/c
	 - m/lcm(a,b) - m/lcm(b,c) - m/lcm(a,c) 
	 + m/lcm(lcm(a,b),c);
	 return cnt >= n;
}
int nthUglyNumber(int n, int a, int b, int c) {
	ll begin =1,end=2e10+5;
	while(begin+1<end){  // 注意是begin+1<end
		int mid = (begin+end)/2;
		if(countLessN(mid,a,b,c,n))
			end = mid;
		else begin=mid;
	}
	
	return end;
	
}
// 测试下
int main()
{
	int n,a,b,c;
	n = 1000000000, a = 2, b = 217983653, c = 336916467;
	cout << nthUglyNumber(n,a,b,c);
 	return 0; 
}

```

## Problem3
### 题目
>给你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。
你可以 任意多次交换 在 pairs 中任意一对索引处的字符。
返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。


- 难度：Medium

### 思路
先根据索引对建立并查集，构成若干个连通分量，每个连通分量内部排序，使其排成最小字典序，然后再将排完序的连通分量复原，就可以得到最小字典序的字符串了。


### 题解
```cpp
#include<bits/stdc++.h>
using namespace std;
vector<int> father,sz;
int find(int x){  // 并查集  查
	return father[x]==x?x:father[x] = find(father[x]);
}
string smallestStringWithSwaps(string s,vector<vector<int> > pairs){
	int n=s.size();
	father.resize(n);
	sz.resize(n); 
	for(int i=0;i<n;i++){  // 初始化 
		father[i] = i;
		sz[i] = 1;
	}
	int n1 = pairs.size();
	for(int i=0;i<n1;i++){
		vector<int> tmp = pairs[i];
		int f1 = find(tmp[0]),f2=find(tmp[1]);
		if(f1!=f2){
			if(sz[f1]<sz[f2]){
				father[f1] = f2;
				sz[f2] += sz[f1];
			}
			else{
				father[f2] = f1;
				sz[f1] += sz[f2];
			}
		}
	}
	vector<vector<char> > charArr(n);  
	vector<vector<int> > posArr(n);
	// 并查集发挥作用 
	for(int i=0;i<n;i++){
		charArr[find(i)].push_back(s[i]);
		posArr[find(i)].push_back(i);	
	}
	// char数组排序 
	for(int i=0;i<charArr.size();i++){
		sort(charArr[i].begin(),charArr[i].end());
	}
	// 把排完序的联通块，合到一起 
	string s1(n,'\0');
	for(int i=0;i<n;i++){
		for(int j=0;j<charArr[i].size();j++){
			s1[posArr[i][j]] = charArr[i][j];
		}
	}
	
	return s1;		
}
// 测试下
int main()
{
	string s="cba";
	vector<vector<int> > v;
	vector<int> s1,s2;
	s1.push_back(0);
	s1.push_back(1);
	s2.push_back(1);
	s2.push_back(2);
	v.push_back(s1),v.push_back(s2);
	cout << smallestStringWithSwaps(s,v) << endl; 
	return 0;
}
```

## Problem4 项目管理
略





