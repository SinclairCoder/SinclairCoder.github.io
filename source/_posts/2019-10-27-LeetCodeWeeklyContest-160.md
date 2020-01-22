---
title:  LeetCodeWeeklyContest-160    
date: 2019-10-27
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

## [题目传送门](https://leetcode-cn.com/contest/weekly-contest-160/problems/find-positive-integer-solution-for-a-given-equation/)
## 找出给定方程的正整数解
签到题的新包装，我没有玩过的船新版本。

```cpp
class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& c, int z) {
        vector<vector<int>> res;
        for(int i=1;i<=1000;i++){
            for(int j=1;j<=1000;j++){
                if(c.f(i,j)<z) continue;
                else if(c.f(i,j)==z) {
                    vector<int> v;
                    v.push_back(i),v.push_back(j);
                    res.push_back(v);
                }
                else break;
                
            }
        }
        return res;
    }
};
```

## 循环码排列
给你两个整数 n 和 start。你的任务是返回任意 (0,1,2,,...,2^n-1) 的排列 p，并且满足：
- p[0] = start
- p[i] 和 p[i+1] 的二进制表示形式只有一位不同
- p[0] 和 p[2^n -1] 的二进制表示形式也只有一位不同

举个例子：
- 输入：n = 2, start = 3
- 输出：[3,2,0,1]
- 解释：这个排列的二进制表示是 (11,10,00,01)
- 所有的相邻元素都有一位是不同的，另一个有效的排列是 [3,1,0,2]

## 思路
参考：
- [[Java/C++/Python] 4-line Gray Code](https://leetcode.com/problems/circular-permutation-in-binary-representation/discuss/414203/JavaC++Python-4-line-Gray-Code)
- [Gray code](https://cp-algorithms.com/algebra/gray-code.html)

其实就是格雷码。
使用位运算操作

## 实现

```cpp
vector<int> circularPermutation(int n, int start) {
        vector<int> res; 
        for(int i=0;i< 1<<n;i++){
            res.push_back(start^i^i>>1);
        }
        return res;
        
    }
```

比赛时找了个格雷码的板子，然后改了改，就过掉了...

```cpp
class Solution {
public:
     vector<int> grayCode(int n){
        vector<int> gray;
        if (n < 1) {
            gray.push_back(0);
            return gray;
        }
        int num = pow(2,n);
        int graycode[n];
        for (int i = 0; i < num; i++) {
            IntToBit(graycode, i, n);
            BitToGray(graycode,n);
            gray.push_back(GrayBitToInt(graycode, n));
        }
        return gray;
    }
    
    void IntToBit(int *code, int n, int bit){
        int i = bit-1;
        while (i >= 0) {
            code[i--] = n%2;
            n/=2;
        }
    }
    
    void BitToGray(int *code, int bit){
        int temp[bit];
        temp[0] = 0^code[0];
        for (int i = 0; i < bit-1; i++) {
            temp[i+1] = code[i]^code[i+1];
        }
        for (int i = 0; i < bit; i++) {
            code[i] = temp[i];
        }
    }
    
    int GrayBitToInt(int *code, int bit){
        int number = 0;
        for (int i = 0; i < bit; i++) {
            if (code[i] == 1) {
                number += pow(2, bit-i-1);
            }
        }
        return number;
    }
    vector<int> circularPermutation(int n, int start) {
        vector<int> res,tmp;
        tmp = grayCode(n);
        int index,msize = pow(2,n);
        for(int i=0;i<msize;i++){
            if(tmp[i]==start){
                index = i;
                break;
            }
        }
        for(int i=index;i<msize;i++){
            res.push_back(tmp[i]);
        }
        for(int i=0;i<index;i++){
            res.push_back(tmp[i]);
        }
        return res;
    }
};
```






## 串联字符串的最大长度

给定一个字符串数组 arr，字符串 s 是将 arr 某一子序列字符串连接所得的字符串，如果 s 中的每一个字符都只出现过一次，那么它就是一个可行解。
请返回所有可行解 s 中最长长度。


## 思路
判定函数+DFS
判定函数：判定一个字符串是否有重复字母，打个大小为26的表即可。
## 实现

```cpp
class Solution {
public:
    int maxL = 0;
    bool pend(string s1){
        int ph[26]={0};
        for(int i=0;i<s1.length();i++){
            if(ph[s1[i]-'a']==1) return false;
            ph[s1[i]-'a']=1;
        }
        return true;
    }
    void dfs(vector<string> arr, int index, string str){
        int len = str.length();
        if(pend(str)) maxL = max(maxL,len);
        if(index == arr.size()||!pend(str)) return ;
        for(int i=index;i<arr.size();i++){
            dfs(arr,i+1,str+arr[i]);
        }
    }
    int maxLength(vector<string>& arr) {
        dfs(arr,0,"");
        return maxL;
    }
};
```



## 铺瓷砖
你是一位施工队的工长，根据设计师的要求准备为一套设计风格独特的房子进行室内装修。
房子的客厅大小为 n x m，为保持极简的风格，需要使用尽可能少的 正方形 瓷砖来铺盖地面。
假设正方形瓷砖的规格不限，边长都是整数。
请你帮设计师计算一下，最少需要用到多少块方形瓷砖？

## 思路
参考：
- [Paper Cut into Minimum Number of Squares | Set 2](https://www.geeksforgeeks.org/paper-cut-minimum-number-squares-set-2/)
- [B站视频讲解](https://www.bilibili.com/video/av73579708?from=search&seid=8107473763828762133)

基本上还是DFS，其实这个题很像经典的贪心题目，不过那个貌似是是个正方形，而且这个题有个特殊的点，就是当n\==11&&m\==13||n\==13&&m==11 这个时候的结果应该为6。剩下的就是先横切，看需要多少方形瓷砖，然后再纵切，看需要多少方形瓷砖，取横切和纵切的最小值，便是结果。

### 实现

```cpp
class Solution {
public:
    int dp[300][300]={0}; 
    int solve(int n,int m){
        if(n==m)
            return 1;
        if(dp[n][m])
            return dp[n][m];
        int res = 2e8;
        for(int i=1;i<=n/2;i++){
            res = min(solve(i,m)+solve(n-i,m),res);
        }
        for(int j=1;j<=m/2;j++){
            res = min(solve(n,j)+solve(n,m-j),res);
        }
        dp[n][m] = res;
        return res;
    }
    int tilingRectangle(int n, int m) {
        if(n==11&&m==13||n==13&&m==11) return 6;
        return solve(n,m);
   
    }
    
};
```



[你一人低头在路上... 多向往  多漫长](https://y.qq.com/portal/player.html)
