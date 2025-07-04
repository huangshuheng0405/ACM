# 图的遍历

## 深搜+广搜

# 【深基18.例3】查找文献

## 题目描述

小 K 喜欢翻看洛谷博客获取知识。每篇文章可能会有若干个（也有可能没有）参考文献的链接指向别的博客文章。小 K 求知欲旺盛，如果他看了某篇文章，那么他一定会去看这篇文章的参考文献（如果他之前已经看过这篇参考文献的话就不用再看它了）。

假设洛谷博客里面一共有 $n(n\le10^5)$ 篇文章（编号为 1 到 $n$）以及 $m(m\le10^6)$ 条参考文献引用关系。目前小 K 已经打开了编号为 1 的一篇文章，请帮助小 K 设计一种方法，使小 K 可以不重复、不遗漏的看完所有他能看到的文章。

这边是已经整理好的参考文献关系图，其中，文献 X → Y 表示文章 X 有参考文献 Y。不保证编号为 1 的文章没有被其他文章引用。

![](https://cdn.luogu.com.cn/upload/image_hosting/f4n4tlhi.png)

请对这个图分别进行 DFS 和 BFS，并输出遍历结果。如果有很多篇文章可以参阅，请先看编号较小的那篇(因此你可能需要先排序)。

## 输入格式

共 $m+1$ 行，第 1 行为 2 个数，$n$ 和 $m$，分别表示一共有 $n(n\le10^5)$ 篇文章（编号为 1 到 $n$）以及$m(m\le10^6)$ 条参考文献引用关系。

接下来 $m$ 行，每行有两个整数 $X,Y$ 表示文章 X 有参考文献 Y。

## 输出格式

共 2 行。
第一行为 DFS 遍历结果，第二行为 BFS 遍历结果。

## 样例 #1

### 样例输入 #1

```
8 9
1 2
1 3
1 4
2 5
2 6
3 7
4 7
4 8
7 8
```

### 样例输出 #1

```
1 2 5 6 3 7 8 4 
1 2 3 4 5 6 7 8
```

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<string>
#include<cmath>
#include<set>
#include<map>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
bool visdfs[N],visbfs[N];
vector<int>adj[N];
void dfs(int u) {
    visdfs[u]=true;
    cout<<u<<' ';
    for(int v:adj[u]) {
        if(visdfs[v]) {
            continue;
        }
        dfs(v);
    }
}
void bfs(int x) {
    queue<int>q;
    q.push(x);
    visbfs[x]=true;
    while(!q.empty()) {
        int u=q.front();
        q.pop();
        cout<<u<<' ';
        for(int v:adj[u]) {
            if(visbfs[v]) {
                continue;
            }
            visbfs[v]=true;
            q.push(v);
        }
    }
}
void solve() {
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=m;i++) {
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
    }
    for(int i=1;i<=n;i++) {
        sort(adj[i].begin(),adj[i].end());
    }
    dfs(1);
    cout<<endl;
    bfs(1);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;
    //cin >> _;
    while (_--){
        solve();
    }
    return 0;
}
```

