---
title: LeetCodeWeeklyContest-157      
date: 2019-10-07
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

秋意寂寥，国庆假期归来，"百废待兴"。
路漫漫其修远兮，吾将上下而求索。

## [题目传送](https://leetcode-cn.com/contest/weekly-contest-157/)


## 玩筹码
数轴上放置了一些筹码，每个筹码的位置存在数组 chips 当中。
你可以对 任何筹码 执行下面两种操作之一（不限操作次数，0 次也可以）：
将第 i 个筹码向左或者右移动 2 个单位，代价为 0。
将第 i 个筹码向左或者右移动 1 个单位，代价为 1。
最开始的时候，同一位置上也可能放着两个或者更多的筹码。
返回将所有筹码移动到同一位置（任意位置）上所需要的最小代价。

### 思路
做的时候看了半天，不会写，后来发现题目理解错了，chips上存放的是筹码的位置，偶数位置跳到偶数位置代价为0，偶数位置跳到奇数位置代价为1，奇数位置跳到奇数位置代价为0，奇数位置跳到偶数位置代价为1，假设奇数位置的筹码数量为 evenN，偶数位置的筹码数量为 oldN，最小代价即为 min(evenN,oldN)

### 实现
```cpp
int minCostToMoveChips(vector<int>& chips) {
        int even=0,odd=0;
        for(int &i:chips){
            if(i%2==0) odd++;
            else even++;
        }
        return min(even,odd);
        
    }
```

## 最长定差子序列
给你一个整数数组 arr 和一个整数 difference，请你找出 arr 中所有相邻元素之间的差等于给定 difference 的等差子序列，并返回其中最长的等差子序列的长度。
举个栗子
输入：arr = [1,5,7,8,5,3,4,2,1], difference = -2
输出：4
解释：最长的等差子序列是 [7,5,3,1]。

### 思路
经典dp问题，不过我的dp确实不熟。
首先打个表dp，用arr[i]作为dp的索引，dp[j]表示到j为止存在的等差子序列长度的最大值。
状态转移方程如下：dp[arr[i]] = dp[arr[i]-d] + 1

### 实现
```cpp
int longestSubsequence(vector<int>& arr, int difference) {
        map<int,int> dp;
        int ans=0;
        for(int i:arr){
            dp[i] = dp[i-difference] + 1;
            ans = max(dp[i],ans);
        }
        return ans;
    }
```

没有冗长的代码，真的爱上了这种感觉，写起来真的既轻松又愉快。

## 黄金矿工 
你要开发一座金矿，地质勘测学家已经探明了这座金矿中的资源分布，并用大小为 m * n 的网格 grid 进行了标注。每个单元格中的整数就表示这一单元格中的黄金数量；如果该单元格是空的，那么就是 0。
为了使收益最大化，矿工需要按以下规则来开采黄金：
每当矿工进入一个单元，就会收集该单元格中的所有黄金。
矿工每次可以从当前位置向上下左右四个方向走。
每个单元格只能被开采（进入）一次。
不得开采（进入）黄金数目为 0 的单元格。
矿工可以从网格中 任意一个 有黄金的单元格出发或者是停止。
### 思路
DFS，其实这里面还是有很多问题要注意一下的，比如dfs函数里的参数tmp不能传引用，而value、visit要传引用，tmp是每次的中间值，每次与value进行比较，用两者的最大值去更新value。
另外，leetcode支持c++11，故此代码里用了很多新特性，比如类的属性定义时初始化，vector初始化等，如果要在Dev上跑的话，在文件首部加上下面这一行：
```cpp
#pragma GCC diagnostic error "-std=c++11" 
```

### 实现
```cpp
class Solution {
public:
    int dx[4]={0,0,-1,1},dy[4]={-1,1,0,0};
    void dfs(vector<vector<int>>& grid, int x, int y, int tmp, int& value, vector<vector<bool>>& visit){  // tmp 不能加引用
        if(grid[x][y]==0) return ;
        value = max(value,tmp += grid[x][y]);
        visit[x][y] = true;
        
        for(int i=0;i<4;i++){
            int tmpx = x+dx[i],tmpy = y+dy[i];
            if(tmpx<0||tmpx>=grid.size()||tmpy<0||tmpy>=grid[0].size()) continue; // 注意等号
            if(grid[tmpx][tmpy]==0||visit[tmpx][tmpy]) continue;
            dfs(grid,tmpx,tmpy,tmp,value,visit);
        }
        visit[x][y] = false;
    }
    int getMaximumGold(vector<vector<int>>& grid) {
        int m=grid.size(), n=grid[0].size(), result = 0;
        // bool visit[m][n]={0};
        vector<vector<bool>> visit(m,vector<bool>(n,false)); // init
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]!=0){
                    dfs(grid,i,j,0,result,visit);
                }
            }
        }
        return result;
    }
};
```



## 统计元音字母序列的数目
给你一个整数 n，请你帮忙统计一下我们可以按下述规则形成多少个长度为 n 的字符串：
字符串中的每个字符都应当是小写元音字母（'a', 'e', 'i', 'o', 'u'）
每个元音 'a' 后面都只能跟着 'e'
每个元音 'e' 后面只能跟着 'a' 或者是 'i'
每个元音 'i' 后面 不能 再跟着另一个 'i'
每个元音 'o' 后面只能跟着 'i' 或者是 'u'
每个元音 'u' 后面只能跟着 'a'
由于答案可能会很大，所以请你返回 模 10^9 + 7 之后的结果。
## 思路一
其实就是一个状态转移方程？
用dp来记录a,e,i,o,u开头的固定长度的字符串对应的方案个数。
初始时dp[i]=1, 其中0<=i<5
进行n次循环的ndp状态数组用来记录此时对应的方案个数
最重要的是根据题目中的字符串相邻规则，找到状态转移方程。
**ndp[0] = dp[1];
ndp[1] = (dp[0]+dp[2])%mod;
ndp[2] = (dp[0]+dp[1]+dp[3]+dp[4])%mod;
ndp[3] = (dp[2]+dp[4])%mod;
ndp[4] = dp[0];**
具体实现看实现一，其中数据类型要用long long ，用int会爆掉。
## 实现一
```cpp
class Solution {
public:
    typedef long long ll;
   	const int mod = 1e9 + 7;
    int countVowelPermutation(int n) {
       ll dp[5]={1,1,1,1,1};
        ll ndp[5]={0};
        for(int i=1;i<n;i++){
            ndp[0] = dp[1];
            ndp[1] = (dp[0]+dp[2])%mod;
            ndp[2] = (dp[0]+dp[1]+dp[3]+dp[4])%mod;
            ndp[3] = (dp[2]+dp[4])%mod;
            ndp[4] = dp[0];
            for(int j=0;j<5;j++) dp[j] = ndp[j];
        }
        ll ans = 0;
        for(int i=0;i<5;i++) ans += dp[i];
        return ans%mod;
    }
};
```
## 思路二
在力扣讨论区看到这样一句话
>这个题数据量再大一些就应该采用 矩阵来做运算，并可以通过矩阵快速幂来加速运算

我的第一反应是根据题目中的字符串相邻规则构造一个邻接矩阵，这个时候离散数学就应该派上用场了，思考邻接矩阵的n次幂的意义为何？实际上，求长度为n的字符串实际上就是求邻接矩阵的n-1次幂，这里需要用到矩阵的幂运算，这个时候就可以使用矩阵的快速幂，网上找了个板子，改了改，没想到就过掉了。
## 实现二
```cpp
const int N=5;
typedef long long ll;
class Matrix{	//矩阵结构体 
    public:
    ll matrix[N][N];
};
class Solution {
public:
   const int mod = 1e9 + 7;
    //初始化为单位矩阵 
   void init(Matrix &res){
        memset(res.matrix,0,sizeof(res.matrix));
        for(int i=0;i<N;i++)
            res.matrix[i][i]=1;
    }
    //矩阵乘法
    Matrix multiplicative(Matrix a,Matrix b){
        Matrix res;
        memset(res.matrix,0,sizeof(res.matrix));
        for(int i = 0 ; i < N ; i++){
            for(int j = 0 ; j < N ; j++){
                for(int k = 0 ; k < N ; k++){
                    res.matrix[i][j] += a.matrix[i][k]*b.matrix[k][j];
                    res.matrix[i][j] %= mod;
                }
            }
        }
        return res;
    }
    //矩阵快速幂 
    Matrix Pow(Matrix mx,ll m){
        Matrix res,base=mx;
        init(res);
        while(m){
            if(m&1)
            res=multiplicative(res,base);
            base=multiplicative(base,base);
            m>>=1;
        }
        return res;
    }
 
    int countVowelPermutation(int n) {
    	if(n==1) return 5;
    	const int MOD = 1e9+7;
        Matrix mat;
        memset(mat.matrix,0,sizeof(mat.matrix));
        mat.matrix[0][1]=1;
        mat.matrix[1][0]=1,mat.matrix[1][2]=1;
        mat.matrix[2][0]=1,mat.matrix[2][1]=1,mat.matrix[2][3]=1,mat.matrix[2][4]=1;
        mat.matrix[3][2]=1,mat.matrix[3][4]=1;
        mat.matrix[4][0]=1;
        Matrix result = Pow(mat,n-1);
        int ans = 0;
        for(int i=0;i<N;i++){
            for(int j=0;j<N;j++){
                ans += result.matrix[i][j];
                ans = ans%MOD;  // 这里要取一次余，不然会爆范围
                // cout << result.matrix[i][j] << " ";
            }
            cout << endl;
        }
        return ans%MOD;    
    }
};
```
 
## 感悟
每写一次周赛题解，都会去看很多大神的代码，学到很多东西，总觉得我的coding水平好像又进步了？ 坏了，是错觉...

 

