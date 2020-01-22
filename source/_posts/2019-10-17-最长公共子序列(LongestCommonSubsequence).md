---
title: 最长公共子序列(LongestCommonSubsequence)     
date: 2019-10-17
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: false
# img: /source/images/xxx.jpg
tags:
    - 最长公共子序列
categories:
	- 动态规划
---

## 问题描述
求两个长度分别为m和n的字符串A、B的最长公共子序列。
A：a<sub>0</sub>a<sub>1</sub>a<sub>2</sub>...a<sub>m-1</sub>
B：b<sub>0</sub>b<sub>1</sub>b<sub>2</sub>...b<sub>n-1</sub>

## 思路
构造一个大小为(m+1)*(n+1)的二维数组dp
dp[i][j] 表示a<sub>0</sub>a<sub>1</sub>a<sub>2</sub>...a<sub>i-1</sub> 和 b<sub>0</sub>b<sub>1</sub>b<sub>2</sub>...b<sub>j-1</sub> 两个字符串的最大公共子序列

### 状态转移方程

$$ dp[i][j]= \begin{cases}0  & \text {if  i=0 or j=0} \\ dp[i-1][j-1]+1   & \text{if a[i-1]=b[j-1] }   \\ max(dp[i-1][j],dp[i][j-1])  &\text{if a[i-1]!=b[j-1]}\end{cases} $$

## 实现
```cpp
int dp[MAXN][MAXN]={0};
vector<char> subs;
vector<char> longestCommonSubsequence(string s1, string s2) {
    memset(dp,0,sizeof(dp));
    int m = s1.length(),n = s2.length();
    for(int i=1;i<=m;i++){
    	for(int j=1;j<=n;j++){
    		if(s1[i-1]==s2[j-1])
    			dp[i][j] = dp[i-1][j-1]+1;
    		else dp[i][j] = max(dp[i-1][j],dp[i][j-1]); 
		}
	}
	int k = dp[m][n],i=m,j=n;
	while(k>0){
		if(dp[i][j]==dp[i-1][j])
			i--;
		else if(dp[i][j]==dp[i][j-1])
			j--;
		else {
 			subs.push_back(s1[i-1]);
			i--;j--;k--;
		}
	}
	return subs;
}
```


来道板子题试试：[1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/submissions/)