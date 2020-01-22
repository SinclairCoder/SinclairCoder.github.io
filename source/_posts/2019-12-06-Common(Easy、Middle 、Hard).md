---
title:  Common (Easy、Middle 、Hard)    
date: 2019-12-06
author: SinclairWang
# img: 
top: false # 是否推荐
cover: true # 是否轮播
# password: 
toc: false
mathjax: true
# img: /source/images/xxx.jpg
tags:
    - 组合数学
    - 暴力
categories:
	- 蓝桥杯
---

一道题
# Easy Version
## 题目描述
$求区间 [l, r] 内是 a 或 b 的倍数的数的个数。$
## Input
$第一行一个正整数 T, 代表测试的组数。$
$之后 T 行，每行四个正整数 a, b, l, r, 以空格分隔，意义如题面所述。$
$1≤T≤100$
$1≤a,b≤10^5$
$1≤l≤r≤10^5$
## Output
$对于每组测试，一行内输出一个整数表示答案。$
## Sample Input
>3
2 3 1 100
4 6 1 100
5 10 1 100

## Sample Output
>67
33
20

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
	
	int l,r,a,b;
	int n;
	cin >> n;
	while(n--){
		cin >> a >> b;
		cin >> l >> r;
		int res = 0;
		for(int i=l;i<=r;i++){
			if(i%a==0||i%b==0) res ++;
		}
		cout << res << endl;	
	}
	return 0;
} 
```

# Middle Version
数据范围改变
$1≤T≤100$
$1≤a,b≤10^9$
$1≤l≤r≤10^9$
需要优化
```cpp
#include<bits/stdc++.h>

using namespace std;
typedef long long ll;

ll gcd(ll a,ll b) { return !b?a:gcd(b,a%b);}
ll lcm(ll a,ll b){ return a/gcd(a,b)*b;}
int main(){
	int n;
	cin >> n;
	while(n--){
		ll a,b,l,r;
		cin >> a >> b;
		cin >> l >> r;
		ll res = 0;
		ll t = lcm(a,b);
		res += r/a+r/b-r/t-(l/a+l/b-l/t);
		if(l%a==0||l%b==0){
			res ++;	
		}
		cout << res << endl;	
	}
	return 0;
} 
```
## Hard Version
$区间[l,r]内, 是a或b倍数的数的和是多少?$
$1≤a,b≤10^9$
$1≤l≤r≤10^9$
```cpp
#include<bits/stdc++.h>

using namespace std;
typedef long long ll;

ll gcd(ll a,ll b) { return !b?a:gcd(b,a%b);}
ll lcm(ll a,ll b){ return a/gcd(a,b)*b;}

int main(){

		ll a,b,l,r;
		cin >> a >> b;
		cin >> l >> r;
		ll res = 0;
		ll t = lcm(a,b);
		ll c1 = r/a,c2 = r/b,c3 = r/t;
		ll c4 = (l-1)/a,c5 = (l-1)/b,c6 = (l-1)/t;
		res += (a+c1*a)*c1/2+(b+c2*b)*c2/2-(t+c3*t)*c3/2;
		res -= (a+c4*a)*c4/2+(b+c5*b)*c5/2-(t+c6*t)*c6/2;
		cout << res << endl;	

	return 0;
} 
```