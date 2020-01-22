---
title:  LeetCodeWeeklyContest-161    
date: 2019-11-03
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


希望你还有诗和远方。

---
<br>

## 传送门： [第 161 场周赛](https://leetcode-cn.com/contest/weekly-contest-161)

## 交换字符使得字符串相同
有两个长度相同的字符串 s1 和 s2，且它们其中 只含有 字符 "x" 和 "y"，你需要通过「交换字符」的方式使这两个字符串相同。
每次「交换字符」的时候，你都可以在两个字符串中各选一个字符进行交换。
交换只能发生在两个不同的字符串之间，绝对不能发生在同一个字符串内部。也就是说，我们可以交换 s1[i] 和 s2[j]，但不能交换 s1[i] 和 s1[j]。最后，请你返回使 s1 和 s2 相同的最小交换次数，如果没有方法能够使得这两个字符串相同，则返回 -1 。
示例：
>输入：s1 = "xx", s2 = "yy"
输出：1
解释：
交换 s1[0] 和 s2[1]，得到 s1 = "yx"，s2 = "yx"。
## 思路
通过找规律可以发现，只有s1 = "xx"，s2 = "yy"或者s1 = "xy", s2 = "yx"这两种情况，分别需要的交换次数为1和2，那只需要统计两个字符串里面有多少个这样的组合。

- 同时扫描两个字符串，如果遇到不相同的字符，就把s1[i]中的字符压入vector（当然s2[i]也可以）
- 然后数vector里面x和y的个数
- 如果x和y的个数为奇数就无法完成交换，返回-1
- x/2+y/2 就是对应s1 = "xx", s2 = "yy"组合的个数
- x%2就是对应s1 = "xy", s2 = "yx"组合的个数
- result = x/2+y/2+(x%2)*2

## 实现

```cpp
#define rep(i,m,n) for(int i=m;i<n;i++)
#define pb push_back
class Solution {
public:
    int minimumSwap(string s1, string s2) {
        vector<char> ans;
        rep(i,0,s1.length()){
            if(s1[i]!=s2[i])
                ans.pb(s1[i]);
        }
        int x=0,y=0;
        rep(i,0,ans.size()){
            if(ans[i]=='x')
                x++;
            else if(ans[i]=='y')
                y++;
            cout << ans[i];
        }
        if((x+y)%2) return -1;
        else return x/2+y/2+(x%2)*2;
        
    }
};
```

## 统计「优美子数组」  
给你一个整数数组 nums 和一个整数 k。
如果某个子数组中恰好有 k 个奇数数字，我们就认为这个子数组是「优美子数组」。
请返回这个数组中「优美子数组」的数目。
示例：
>输入：nums = [1,1,2,1,1], k = 3
输出：2
解释：包含 3 个奇数的子数组是 [1,1,2,1] 和 [1,2,1,1] 。

## 思路
暴力会超时。
主要还是做一个数组，统计从第一个元素开始到现在奇数的个数。然后使用upper_bound 和 lower_bound
更多解法请参考：[1248. Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/discuss/?currentPage=1&orderBy=hot&query=)
## 实现

```cpp
#define rep(i,m,n) for(int i=m;i<n;i++)
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        int res = 0;
        int cnt[50005];
        memset(cnt,0,sizeof(cnt));
        rep(i,0,nums.size()){
            if(nums[i]%2){
                cnt[i+1] = cnt[i]+1; 
            }
            else cnt[i+1] = cnt[i];
        }  
        rep(i,1,nums.size()+1){
            res += upper_bound(cnt+1,cnt+nums.size()+1,cnt[i-1]+k) - lower_bound(cnt+1,cnt+nums.size()+1,cnt[i-1]+k);
        }
        return res;
    }
};


```

不过发现这种解法虽然运行没有问题，但是自己手动拿纸推的时候貌似样例过不去。
upper_bound 找的是范围内第一个大于x的位置
 lower_bound找的是范围内第一个大于等于x的位置
**nums = [2,2,2,1,2,2,1,2,2,2], k = 2**这个样例

|nums| 2 | 2 | 2  |1 | 2  | 2 | 1  | 2 |  2 | 2 |
|--|--|--|--|--|--|--|--|--|--|--|
|index  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|dict  | 0 | 0 | 0 | 1 | 1 | 1 | 2 | 2 | 2 | 2 |
|upper  | 10 | 10 | 10 | 10 | 10 | 10 |10 | 10| 10 | 10 |
|lower  | 6 | 6 | 6 | 10 | 10 | 10 |10 |10| 10|10 |

**这样一算 结果等于(10-6)*3=12!=16  这是为甚麽？***


## 移除无效的括号
给你一个由 '('、')' 和小写字母组成的字符串 s。
你需要从字符串中删除最少数目的 '(' 或者 ')' （可以删除任意位置的括号)，使得剩下的「括号字符串」有效。请返回任意一个合法字符串。
有效「括号字符串」应当符合以下 任意一条 要求：
- 空字符串或只包含小写字母的字符串
- 可以被写作 AB（A 连接 B）的字符串，其中 A 和 B 都是有效「括号字符串」
- 可以被写作 (A) 的字符串，其中 A 是一个有效的「括号字符串」
### 思路
一开始的想法是用栈来做，可是无法记录该删去的字符的下标，想用stack<pair<char,int> >来做，不知道为甚麽，好像是指针有问题。
其实用数组模拟就可以，第一遍先正向扫描，统计和记录‘)’，删去多余的‘)’，记录多余的'('的个数cnt
，然后此时可能'('的个数刚好和‘)’的个数配对，也可能有多的，此时需要再逆向扫描一遍字符串，然后去掉多余的前cnt个字符
### 实现

```cpp
#define rep(i,m,n) for(int i=m;i<n;i++)
#define rep1(i,m,n) for(int i=m;i>=n;i--)
class Solution {
public:
    string minRemoveToMakeValid(string s) {
       int cnt = 0;
        string res="";
        rep(i,0,s.length()){
            if(s[i]=='(')
                cnt++,res+=s[i];
            else if(s[i]==')'){
        	    if(cnt) cnt--,res+=s[i];
            }
            else res += s[i];
        }   
        string rres = "";
        rep1(i,res.length()-1,0){
            if(cnt&&res[i]=='(')
                cnt--;
            else rres += res[i];
        }
        reverse(rres.begin(),rres.end());
        return rres;
        
    }
};
```

## 检查「好数组」
给你一个正整数数组 nums，你需要从中任选一些子集，然后将子集中每一个数乘以一个 任意整数，并求出他们的和。
假如该和结果为 1，那么原数组就是一个「好数组」，则返回 True；否则请返回 False。
示例：
>输入：nums = [12,5,7,23]
输出：true
解释：挑选数字 5 和 7。
5*3 + 7*(-2) = 1

### 思路
虽然写题目的时候一眼就看出是欧几里得算法，只要数组里面存在若干个数互质即为好数组，也即gcd(a,b,c...)=1，后来越写越复杂，实现思路跑偏了...

### 实现

```cpp
class Solution {
public:
    int gcd(int m,int n){
	    return n ? gcd(n,m%n): m; 
    }
    bool isGoodArray(vector<int>& nums) {
        int res = nums[0];
        for(int a:nums){
            res = gcd(res,a);
        }
        if(res==1) return true;
        else  return false;
    }
};
```