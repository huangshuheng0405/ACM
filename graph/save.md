# 图的存储

## 题目描述

给定一个 $n$ 个顶点 $m$ 条边的无向图。请以邻接矩阵和邻接表的形式输出这一张图。

## 输入格式

第一行输入两个正整数 $n$ 和 $m$，表示图的顶点数和边数。

第二行开始，往后 $m$ 行，每行输入两个以空格隔开的正整数 $u,v$，表示 $u,v$ 顶点之间有一条边直接相连。

## 输出格式

首先输出 $n$ 行 $n$ 列的矩阵，以空格隔开每一行之间的数表示邻接矩阵。第 $i$ 行第 $j$ 列的数为 $1$ 则表示顶点 $i,j$ 之间有一条边直接相连；若为 $0$ 则表示没有直接相连的边。

再往后输出 $n$ 行。第 $i$ 行首先先输出一个整数 $d_i$，表示这个顶点的度数，再按照从小到大的顺序，依次输出与顶点 $i$ 直接相连的所有顶点。

## 样例 #1

### 样例输入 #1

```
5 5
1 2
2 3
3 5
1 3
3 4
```

### 样例输出 #1

```
0 1 1 0 0
1 0 1 0 0
1 1 0 1 1
0 0 1 0 0
0 0 1 0 0
2 2 3
2 1 3
4 1 2 4 5
1 3
1 3
```

## 提示

样例的图如图所示：

![](https://cdn.luogu.com.cn/upload/image_hosting/s78y6bsb.png)

数据保证，对于所有数据，$1 \leq n \leq 1000$，$1 \leq m \leq 10^5$，且图无重边无自环。

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
const int N = 1e3 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
vector<int>adj[N];
int a[N][N];
void solve() {
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=m;i++) {
        int u,v;
        cin>>u>>v;
        a[u][v]=a[v][u]=1;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=n;j++) {
            cout<<a[i][j]<<' ';
        }
        cout<<endl;
    }
    for(int i=1;i<=n;i++) {
        cout<<adj[i].size()<<' ';
        sort(adj[i].begin(),adj[i].end());
        for(int v:adj[i]) {
            cout<<v<<' ';
        }
        cout<<endl;
    }
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

# 链式前向星(Link Forward Star)

用于对**边**进行查找或操作

头插法，后插入的边先访问

### 数组

```c++
void add(int a,int b,int c){
    to[idx]=b;//to存a指向的点
    w[idx]=c;//w存a到b的边权
    ne[idx]=h[a];//下一条边
    h[a]=idx++;//h[a]存储的是a点的第一条出边的编号
}
//初始化数组
memset(h,-1,sizeof(h));//头文件<cstring>
idx=0;
//建边 
add(u,v,w);
add(v,u,w);
//遍历
for(int i=h[u];i!=-1;i=ne[i]){
    int v=to[i];//v是u的邻点
}
```

### 结构体数组

```c++
const int N=1e5;
struct edge{
    int to,w,ne;
}Edge[N];//双向边要乘2
int cur,h[N];
void add(int u,int v,int w){//建边
	adj[++cnr]={v,w,h[u]};
    h[u]=cur;
}//边的编号从1到idx
for(int i=h[u];i;i=Edge[i].ne){
    int v=Edge[i].to;//u的邻点
    
}
```

## 邻接表(Adjacency List)

```c++
struct edge{
    int v,w;
};
vector<edge>e[MAXSIZE];
e[u].push_back({v,w});
e[v].push_back({u,w});
for(auto v:e[u]){//枚举u的邻点v
    
}
```

