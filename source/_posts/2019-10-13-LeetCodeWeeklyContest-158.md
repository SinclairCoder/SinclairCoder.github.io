---
title: LeetCodeWeeklyContest-158    
date: 2019-10-13
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

戒骄戒躁，勇往直前。
<br>
## 传送门：[第 158 场周赛](https://leetcode-cn.com/contest/weekly-contest-158)
## 分割平衡字符串
在一个「平衡字符串」中，'L' 和 'R' 字符的数量是相同的。
给出一个平衡字符串 s，请你将它分割成尽可能多的平衡字符串。
返回可以通过分割得到的平衡字符串的最大数量。
输入：s = "RLRRLLRLRL"
输出：4
解释：s 可以分割为 "RL", "RRLL", "RL", "RL", 每个子字符串中都包含相同数量的 'L' 和 'R'。
### 思路
用两个数组记录当前出现的R和L的次数，进行比对相等则cnt+1。
### 实现
```cpp
class Solution {
public:
    int balancedStringSplit(string s) {
        int n = s.length(),cnt=0;
        int rcnt[n]={0},lcnt[n]={0};
        for(int i=0;i<s.length();i++){
            rcnt[i]=(i==0?0:rcnt[i-1]);
            lcnt[i]=(i==0?0:lcnt[i-1]);
            if(s[i]=='R')  rcnt[i]++;
            if(s[i]=='L')  lcnt[i]++;
            if(lcnt[i]==rcnt[i]) cnt++; 
        }
        return cnt;
    }
};
```

##  可以攻击国王的皇后
在一个 8x8 的棋盘上，放置着若干「黑皇后」和一个「白国王」。
「黑皇后」在棋盘上的位置分布用整数坐标数组 queens 表示，「白国王」的坐标用数组 king 表示。
「黑皇后」的行棋规定是：横、直、斜都可以走，步数不受限制，但是，不能越子行棋。
请你返回可以直接攻击到「白国王」的所有「黑皇后」的坐标（任意顺序）。
### 思路
第一反应想起了8皇后，基本上还是以国王为中心遍历8个方向，在一个方向碰到一个皇后，就立刻停止，push进result，然后遍历剩余的方向。
注意：vector数组的初始化。
### 实现
```cpp
class Solution {
public:
    vector<vector<int>> queensAttacktheKing(vector<vector<int>>& queens, vector<int>& king) {
        vector<vector<int> > res;
        vector<int> tmp(queens[0].size()); // 要初始化 不然报 reference binding to null pointer of type ‘value_type’
        vector<vector<int> >::iterator it;
        int dx[8]={-1,0,1,-1,1,-1,0,1};
        int dy[8]={-1,-1,-1,0,0,1,1,1};
        for(int i=0;i<8;i++){
            tmp[0]=king[0],tmp[1]=king[1];
            while(1){
                tmp[0] = tmp[0]+dx[i];
                tmp[1] = tmp[1]+dy[i];
                if(tmp[0]>=0&&tmp[0]<8&&tmp[1]>=0&&tmp[1]<8){
                    it = find(queens.begin(),queens.end(),tmp);  // 最近用find老是出错
                    if(it!=queens.end()){
                        res.push_back(tmp);
                        break;
                    }
                }
                else break;      
            }                      
        }
        return res;
        
    }
};

```

## 掷骰子模拟
有一个骰子模拟器会每次投掷的时候生成一个 1 到 6 的随机数。
不过我们在使用它时有个约束，就是使得投掷骰子时，连续 掷出数字 i 的次数不能超过 rollMax[i]（i 从 1 开始编号）。
现在，给你一个整数数组 rollMax 和一个整数 n，请你来计算掷 n 次骰子可得到的不同点数序列的数量。
假如两个序列中至少存在一个元素不同，就认为这两个序列是不同的。由于答案可能很大，所以请返回 模 10^9 + 7 之后的结果。

### 思路1
请参考[ \[Chinese/C++\] 1223. 【动态规划】有限制的掷骰子（详解）](https://leetcode.com/problems/dice-roll-simulation/discuss/403736/ChineseC++-1223.)
### 实现1
```cpp
class Solution{
    vector<int> rollMax ;
    typedef long long ll;
    int dp[5005][6][16];
    int mod = 1e9+7;
    ll dfs(int num,int k,int n){
        if(n==0) return 1;
        if(dp[n][num][k]!=-1) return dp[n][num][k];
        
        ll ans =0;
        for(int i=0;i<6;i++){
            if(i!=num) ans += dfs(i,1,n-1);
            else if(k<rollMax[i])   // 注意不能等于,如果等于就不能加一了。
                ans += dfs(num,k+1,n-1);
            else ;
        }
        return dp[n][num][k]=ans%mod;
    }
    
public :
    int  dieSimulator(int n, vector<int>& rollMax) {
        this->rollMax = rollMax;
        memset(dp,-1,sizeof(dp));
        return dfs(0,0,n)%mod;
    }
};
```
### 思路2
用一个三维数组表示的话。v[k][i][j]表示第k轮扔出i点的骰子，并且这个点数已经连续出现j次时的序列的个数。
 如果下一轮的点数和这一轮最后的点数不同的时候：v[k+1][c][1] += v[k][i][j]
如果下一轮的点数和这一轮最后的点数相同并且依然不超过这个点最大的可连续限制的时候：v[k+1][i][j+1] += v[k][i][j]

不过尝试实现了一下，一直显示结果为0，不清楚为什么。欢迎讨论。
```cpp
class Solution {
public:
    typedef long long ll;
    int  dieSimulator(int n, vector<int>& rollMax) {
        int mod = 1e9+7;
        int dp[5002][8][16]={0};
        for(int i=0;i<6;i++){
            dp[1][i][1]=1;
        }
        for(int k=2;k<=n;k++){
            for(int i=0;i<6;i++){
                for(int j=1;j<k&&j<16;j++){
                    for(int c=0;c<6;c++){
                        if(c!=i) dp[k+1][c][1] = (dp[k+1][c][1]+dp[k][i][j])%mod;
                        else if(j<=rollMax[i]) dp[k+1][i][j+1] = (dp[k+1][i][j+1]+dp[k][i][j])%mod;
                    }
                }
            }
        }
        ll res = 0;
        for(int i=1;i<6;i++){
            for(int j=1;j<n;j++){
                res = (res+dp[n][i][j])%mod;
            }
        }
        return res;
        
    }
};

```

## 最大相等频率
给出一个正整数数组 nums，请你帮忙从该数组中找出能满足下面要求的 最长 前缀，并返回其长度：
从前缀中 删除一个 元素后，使得所剩下的每个数字的出现次数相同。
如果删除这个元素后没有剩余元素存在，仍可认为每个数字都具有相同的出现次数（也就是 0 次）。
举个例子
输入：nums = [2,2,1,1,5,3,3,5]
输出：7
解释：对于长度为 7 的子数组 [2,2,1,1,5,3,3]，如果我们从中删去 nums[4]=5，就可以得到[2,2,1,1,3,3]，里面每个数字都出现了两次。
### 思路
参考题解：
-  [Chinese/C++ \] 1224. 【统计】最大相等频率，详解，O(N)](https://leetcode.com/problems/maximum-equal-frequency/discuss/403779/ChineseC++-.-O%28N%29)
- [C++,O(n),consider for four situations](https://leetcode.com/problems/maximum-equal-frequency/discuss/403678/C++O%28n%29consider-for-four-situations)

打两次表，cnt表是用来记录每种数字出现的次数，freq表记录每个次数出现的频率，其中需要考虑四种情况。

1.  只有频率为A或B的序列，其中A=B+1，比如：[7,7,7,8,8,9,9] 其中A=3，B=2
2.  只有频率为A或B的序列，其中A=1，B!=1 比如：[6,7,7,7,8,8,8,9,9,9] 其中A=1,B=3
3.  每个数字只出现1次 比如：[7,8,9] 
4. 所有数字相同，比如：[7,7,7,7,7]

**对于情况1，4**：1+freq[maxcnt-1]*(maxcnt-1)==i+1 其中i为nums的当前索引，等式的意义是 从索引等于0到索引等于i之间的元素个数相等。
**对于情况2**：1+freq[maxcnt]*maxcnt ==i+1 等式意义同上。
**上述三种情况** 所求结果即此时索引i加1（i>=0&&i<nums.size()）
**对于情况3**：只需 maxcnt ==1，所求结果即为整个nums的长度。

### 实现
```cpp
class Solution {
public:
    int maxEqualFreq(vector<int>& nums) {
       vector<int> cnt(100003,0),freq(100003,0);
        int maxcnt = 0,ans = 0;
        for(int i=0;i<nums.size();i++){
            int num = nums[i];
            cnt[num]++;
            
            freq[cnt[num]]++;
            maxcnt = max(maxcnt,cnt[num]);
            // cout << num << " " << cnt[num] << " " <<  freq[cnt[num]] << " " << maxcnt << endl; 
            if(1+freq[maxcnt-1]*(maxcnt-1)==i+1||1+freq[maxcnt]*maxcnt==i+1) 
                ans = i+1;         
        }
        if(maxcnt==1) ans = nums.size();
        return ans;
    }
};
```

写这次的题解并没有想象中的欢快~

所以动心忍性，曾益其所不能。