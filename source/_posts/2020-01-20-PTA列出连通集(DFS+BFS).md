---
title:     PTA列出连通集(DFS+BFS) 
date: 2020-01-20
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: false
# img: /source/images/xxx.jpg
tags:
    - PTA
    - DFS
    - BFS
categories:
	- PTA
---



给定一个有N个顶点和E条边的无向图，请用DFS和BFS分别列出其所有的连通集。假设顶点从0到N−1编号。进行搜索时，假设我们总是从编号最小的顶点出发，按编号递增的顺序访问邻接点。

## 输入格式:
输入第1行给出2个整数N(0<N≤10)和E，分别是图的顶点数和边数。随后E行，每行给出一条边的两个端点。每行中的数字之间用1空格分隔。

## 输出格式:
按照"{ v​1 v​2​​  ... v​k​​  }"的格式，每行输出一个连通集。先输出DFS的结果，再输出BFS的结果。

## 输入样例:
>8 6
0 7
0 1
2 0
4 1
2 4
3 5
  
## 输出样例:
>{ 0 1 4 2 7 }
{ 3 5 }
{ 6 }
{ 0 1 2 7 4 }
{ 3 5 }
{ 6 }


<br>


<br>

```cpp
#include<iostream>
#include<cstring>
#include<vector>
#include<algorithm>
#include<stack>
#include<queue>
#include<map>
#include<list>
#include<set>
#include<cmath>
using namespace std;
typedef long long ll;
#define N 15
int edge[N][N],vis[N];
queue<int> qu;

void dfs(int x,int n){
	vis[x] = 1;
	cout << " " << x;
	for(int i=0;i<n;i++){
		if(edge[x][i]&&!vis[i]) dfs(i,n);
	}
}
void bfs(int x,int n){
	qu.push(x);
	vis[x] = 1;
	while(!qu.empty()){
		int root = qu.front();
		cout << " " << root;
		qu.pop();
		for(int i=0;i<n;i++){
			if(edge[root][i]&&!vis[i]){
				qu.push(i);
				vis[i] = 1;
			}
		}
	}	
}
int main(){
	int n,e,x,y;
	cin >> n >> e;
	for(int i=1;i<=n;i++) {
		edge[i][i] = 1; 
	}
	for(int i=0;i<e;i++){
		cin >> x >>y;
		edge[x][y] = 1;
		edge[y][x] = 1;
	}
	
	for(int i=0;i<n;i++) {
		if(!vis[i]){
			cout << "{";
			dfs(i,n);
			cout << " }" << endl;
		}	
	}
	memset(vis,0,sizeof(vis));
	for(int i=0;i<n;i++){
		if(!vis[i]) {
			cout << "{";
			bfs(i,n);
			cout << " }" << endl;
		}
	}
	return 0;
}

```