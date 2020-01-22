---
title: LeetCodeWeeklyContest-159     
date: 2019-10-21
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

最近看了篇文章，文章里说 **希望你身边能有个比你聪明五倍，但却比你还努力十倍的人。**
倍数虽然有些夸张，但是这个思想还是能get到的。
## 5230. 缀点成线
在一个 XY 坐标系中有一些点，我们用数组 coordinates 来分别记录它们的坐标，其中 coordinates[i] = [x, y] 表示横坐标为 x、纵坐标为 y 的点。

请你来判断，这些点是否在该坐标系中属于同一条直线上，是则返回 <font color="#c7254e" face="Menlo, Monaco, Consolas, Courier New, monospace">true</font>，否则请返回 <font color="#c7254e" face="Menlo, Monaco, Consolas, Courier New, monospace">false</font>。
### 思路
根据前两个点构造直线，看后续的点是否在直线上。
直线方程为  $ax+by+c=0$，其中：$a=y_2-y_1$ $b=x_1-x_2$ $c=-ax_1-by_1$
**好处是不用考虑斜率，这是个方程的一般式。如果考虑斜率，还得分斜率是否为0。**

### 实现
```cpp
class Solution {
public:
    bool checkStraightLine(vector<vector<int>>& coordinates) {
        vector<int> v1,v2,v3;
        v1 = coordinates[0];
        v2 = coordinates[1];
        int a = v2[1]-v1[1];
        int b = v1[0]-v2[0];
        int c = -1*a*v1[0]-b*v1[1];
        for(int i=1;i<coordinates.size();i++){
            v3 = coordinates[i];
            if(a*v3[0]+b*v3[1]+c!=0) return false; 
        }
        return true;
    }
};
```

## 删除子文件夹
你是一位系统管理员，手里有一份文件夹列表 folder，你的任务是要删除该列表中的所有 子文件夹，并以 任意顺序 返回剩下的文件夹。

我们这样定义「子文件夹」：

如果文件夹 folder[i] 位于另一个文件夹 folder[j] 下，那么 folder[i] 就是 folder[j] 的子文件夹。
文件夹的「路径」是由一个或多个按以下格式串联形成的字符串：

/ 后跟一个或者多个小写英文字母。
例如，/leetcode 和 /leetcode/problems 都是有效的路径，而空字符串和 / 不是。

### 思路
- 写一个判别函数来判断是否为子路径
- 将文件夹序列sort，此时字典序小的应该在前，长度小的也在前，故一般父路径也在前。
- 设置一个标记数组，初始置0，如果该文件夹路径要被删除置1。
- 扫描文件夹序列，两两对比，神奇的是不用二重循环就可以做到。
- 将标记为0的push进vector，返回。
### 实现
```cpp
class Solution {
public:
    // a.length < b.length
    bool isSubFolder(string a,string b) {
        int n1 = a.length(),n2 = b.length();
        if(n1>n2) return false;
        int i;
        for(i=0;i<n1;i++){
            if(a[i]!=b[i]) return false;
        }
        if(b[i]!='/') return false;
        return true;        
    }
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(),folder.end());
        int n = folder.size();
        int pend[n]={0}; // delete 1
        int j=0;
        for(int i=1;i<n;i++){
            if(isSubFolder(folder[j],folder[i])){
                pend[i] = 1;
            }
            else j = i;  // important
        }
        vector<string> res;
        for(int i=0;i<n;i++){
            if(!pend[i]) res.push_back(folder[i]);
        }
        
        return res;
        
    }
};
```

## 替换子串得到平衡字符串
有一个只含有 'Q', 'W', 'E', 'R' 四种字符，且长度为 n 的字符串。
假如在该字符串中，这四个字符都恰好出现 n/4 次，那么它就是一个「平衡字符串」。
给你一个这样的字符串 s，请通过「替换子串」的方式，使原字符串 s 变成一个「平衡字符串」。
你可以用和「待替换子串」长度相同的 任何 其他字符串来完成替换。
请返回待替换子串的最小可能长度。
如果原字符串自身就是一个平衡字符串，则返回 0。


### 思路
参考：
- [ lee215 的视频讲解](https://www.youtube.com/watch?v=omnSO-CSFIs)
- [[Java/C++/Python] Sliding Window](https://leetcode.com/problems/replace-the-substring-for-balanced-string/discuss/408978/JavaC++Python-Sliding-Window)

我觉得题目需要重新阅读并理解一下，**题目的意思是在给出的序列中找一个子串，只需要修改这个子串的字符就可以使得这个字符串变为平衡字符串，求这个子串的最小长度。**

思路是lee215 这位大牛的，上面的参考一个是他的视频讲解，另一个是他写的题解。
下面写一下他的思路：
首先扫描一下字符串，把W、E、R、Q这四个字符的个数记录下来，使用**滑动窗口**的思想，从下标0开始，扫描字符串，只考虑滑动窗口以外的字符，不考虑滑动窗口内的字符，滑动窗口外的W、E、R、Q这四个字符的个数，如果字符个数小于等于n/4，则说明改滑动窗口所在的区间越正确，也即滑动窗口内的字符是多余的并需要被修改的，然后找到这个滑动窗口长度的最小值即可。

当然这个算法是O(n)的，代码很简介，或许是全网最短，也或许最难懂。

更多题解可以参考 leetcode英文社区下关于这道题的讨论。[1234. Replace the Substring for Balanced String](https://leetcode.com/problems/replace-the-substring-for-balanced-string/discuss/?currentPage=1&orderBy=hot&query=)

### 实现

```cpp
class Solution {
public:
    int balancedString(string s) {
        // QWER count;
        int n = s.length(),m=n/4,res=n,i=0;
        map<char,int> count;
        for(int j=0;j<n;j++) 
            count[s[j]]++;
        for(int j=0;j<n;j++){
            count[s[j]]--;
            while(i<n&&count['Q']<=m&&count['E']<=m&&count['R']<=m&&count['W']<=m){
                res = min(res,j-i+1);
                count[s[i]]++;
                i++;
            }
        }
        return res;
    }
};
```

## 规划兼职工作
你打算利用空闲时间来做兼职工作赚些零花钱。
这里有 n 份兼职工作，每份工作预计从 startTime[i] 开始到 endTime[i] 结束，报酬为 profit[i]。
给你一份兼职工作表，包含开始时间 startTime，结束时间 endTime 和预计报酬 profit 三个数组，请你计算并返回可以获得的最大报酬。
注意，时间上出现重叠的 2 份工作不能同时进行。
如果你选择的工作在时间 X 结束，那么你可以立刻进行在时间 X 开始的下一份工作。
### 思路一
参考：

- [[Java/C++/Python] DP Solution](https://leetcode.com/problems/maximum-profit-in-job-scheduling/discuss/409009/JavaC++Python-DP-Solution) 
- [ lee215 的视频讲解](https://www.youtube.com/watch?v=omnSO-CSFIs)

再开辟一块空间，把endTime[i],startTime[i],profit[i] 一起存放进去，按照结束时间排序。
然后进行动态规划，使用map，其中dp[i]表示从0到i时间段内的收益，i为结束时间，迭代过程是在dp中找到开始时间大于每次的startTime的结束时间的前一个，也就是小于startTime的最大的endTime,然后与当前的收益相加，最后map的最后一个就是答案。（其实这里有很多细节需要仔细钻研一下，比如之前就踩过的坑，涉及到了map的存储结构等)


### 实现一
```cpp
class Solution {
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = endTime.size();
        vector<vector<int> > jobs;
        for(int i=0;i<n;i++){
            jobs.push_back({endTime[i],startTime[i],profit[i]});
        }
        sort(jobs.begin(),jobs.end());
        map<int,int> dp;
        dp[0] = 0;
        for(auto job:jobs){
            int cur = prev(dp.upper_bound(job[1]))->second + job[2];
            if(cur > dp.rbegin()->second)
                dp[job[0]] = cur;
        }
        return dp.rbegin()->second;
        
    }
};
```



## 思路二
参考：

- [\[C++\]  Sort and DP, O(NlogN)](https://leetcode.com/problems/maximum-profit-in-job-scheduling/discuss/409031/C++-Sort-and-DP-O%28NlogN%29)<br>
不去重新分配存储空间去存储starttime、endTime、profit，只需要新建一个索引数组，自定义排序规则，按照结束时间排序。然后还定义了一个匿名函数，返回在endTime在startTime[i]之前的索引。
然后就是动态规划，dp[i]表示在i时刻及以前所获得的最大收益，cur为当前时刻可以获得的收益。
dp[i] = max(dp[i-1],cur)，最后dp[n-1]便是答案。

更多题解可以参考 leetcode英文社区下关于这道题的讨论。[1235. Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/discuss/?currentPage=1&orderBy=hot&query=)
### 实现二
```cpp
class Solution {
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = startTime.size();
        vector<vector<int>> jobs;
        int index[n];
        for(int i=0;i<n;i++){
            index[i]=i;
        }
        sort(index,index+n,[&endTime](int i,int j){return endTime[i]<endTime[j];});
        auto lastestValidIndex = [&startTime,&endTime,&index](int i){   // lambda
            int start = startTime[index[i]];
            int left = 0, right = i;
            while(left<right){
                int mid = (left+right)>>1;
                if(endTime[index[mid]]<=start)
                    left = mid+1; // +1
                else right = mid;
            }
            return right==0?-1:right-1;
        };
        int dp[n]={0};
        dp[0] = profit[index[0]];
        for(int i=1;i<n;i++){
            int cur = profit[index[i]];
            int last = lastestValidIndex(i);
            if(last!=-1) cur +=dp[last];
            dp[i] = max(dp[i-1],cur);
        }        
        return dp[n-1];
        
    }
};
```
被DP教育4小时。为伊消得人憔悴..
说实话还是leetcode英文社区那边大牛多，讨论也更多一些。

今日份荐读：

- [姚班学霸陈立杰：16岁保送清华，18岁拿下IOI世界冠军，现摘得FOCS 2019最佳学生论文
量子位](https://zhuanlan.zhihu.com/p/87460515)


