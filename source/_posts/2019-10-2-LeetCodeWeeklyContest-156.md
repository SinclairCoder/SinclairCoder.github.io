---
title: LeetCodeWeeklyContest-156     
date: 2019-10-2
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

[题目传送](https://leetcode-cn.com/contest/weekly-contest-156/)
写题解就像写博客一样真的有好处，尽管会多花点时间。
## 1 、独一无二的出现次数
### 描述
给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。
如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false。
### 思路
统计出现次数，然后排序。有相同的为false，否则为true。
### 实现
```c
 bool uniqueOccurrences(vector<int>& arr) {
        int a[2001]={0};
        for(int i=0;i<arr.size();i++){
            a[arr[i]+1000]++;
        }
        sort(a,a+2001);
        for(int i=0;i<2000;i++){
            if(a[i]==a[i+1]&&a[i]!=0){
                return false;
            }
        }
        return true;
        
    }

```
时间复杂度：O(nlog<sub>2</sub>n)

## 2、尽可能使字符串相等
### 描述
给你两个长度相同的字符串，s 和 t。
将 s 中的第 i 个字符变到 t 中的第 i 个字符需要 |s[i] - t[i]| 的开销（开销可能为 0），也就是两个字符的 ASCII 码值的差的绝对值。
用于变更字符串的最大预算是 maxCost。在转化字符串时，总开销应当小于等于该预算，这也意味着字符串的转化可能是不完全的。
如果你可以将 s 的子字符串转化为它在 t 中对应的子字符串，则返回可以转化的最大长度。
如果 s 中没有子字符串可以转化成 t 中对应的子字符串，则返回 0。

### 思路
**官方题解说这是本场比赛里面最好的题目了。**
使用滑动窗口思想。
一般使用滑动窗口思想，求解一个序列中连续k个数的最值问题
 
 下面以s="pxezla",t="loewbi",maxCost=25为例
 
|index|0 |1 |2| 3| 4| 5|
|--|--|--|--|--|--|--|
|string1| p |x | e|z| l|a |
| string2|l|o|e|w|b|i|
|\|cost[i]\||4|9|0|3|10|8|
|\|sum[i]\||4|13|13|16|26|34|

分别使用left，right表示cost数组的左右索引。
left=0,right=0
只要curCost小于maxCost,就right++
当出现maxCost<curCost时，则需要使left--，即向右整体滑动，貌似有点改进的kmp算法的感觉。
最大长度best = max(best,right-left+1);
只需滑动一次，即得最大值，对应的cost数组段为[9 0 3 10]
### 实现一
```c
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {
        vector<int> c;
        for(int i=0;i<s.size();i++){
            c.push_back(abs(s[i]-t[i]));
        }
        int curCost = 0,cnt=0;
        for(int i=0,j=0;i<s.size();i++){
            curCost += c[i];
            while(curCost>maxCost){
                curCost -= c[j];
                j++;
            }
            cnt = max(cnt,i-j+1);
        }
        return cnt;
        
    }
};
```

### 实现二
```c
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {
        vector<int> sumArr;
        int ans =0;
        sumArr.push_back(ans); // 很关键的处理
        for(int i=0;i<s.size();i++){
            ans += abs(s[i]-t[i]);
            // cout << ans << endl;
            sumArr.push_back(ans);
        }
        int curCost = 0,cnt=0;
        for(int i=1,j=0;i<=s.size();i++){  // 注意这里的等号，仔细想想为甚麽？ 
           for(;sumArr[i]-sumArr[j]>maxCost;j++);
            cnt = max(cnt,i-j);
        }
        return cnt;
        
    }
};

```
时间复杂度：O(n)
## 3、删除字符串中的所有相邻重复项 II
### 描述
给你一个字符串 s，「k 倍重复项删除操作」将会从 s 中选择 k 个相邻且相等的字母，并删除它们，使被删去的字符串的左侧和右侧连在一起。
你需要对 s 重复进行无限次这样的删除操作，直到无法继续为止。
在执行完所有删除操作后，返回最终得到的字符串。
本题答案保证唯一。

### 思路
参考官方题解，维护一个栈，栈元素类型为 pair<char,int> ，其中char类型为对应的字符串的s[i]，int类型为对应的个数，由于本题保证答案唯一，题目大大简化，所以最后个数等于k的字符一定可以被删掉。
扫描字符串s，使字符依次进栈，若栈顶的字母对应的个数等于k，便出栈k次。

### 实现

```cpp
class Solution {
public:

string removeDuplicates(string s, int k) {
		stack<pair<char,int> > st;
        for(int i=0;i<s.size();i++){
            if(st.empty()) st.push(make_pair(s[i],1));
            else {
                if(st.top().first==s[i]){
                    st.push(make_pair(s[i],st.top().second+1));
                }
                else st.push(make_pair(s[i],1));
            }
            if(st.top().second==k){
                for(int i=0;i<k;i++){
                    st.pop();
                }
            }
        }
        string result;
        while(st.size()){
            result += st.top().first;
            st.pop();
        }
        reverse(result.begin(),result.end());
        return result; 
    
        
    }

};
```
时间复杂度：O(n)


## 4、穿过迷宫的最少移动次数
略
