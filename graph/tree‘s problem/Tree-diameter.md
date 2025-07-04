# 树的直径（tree-diameter）

## 题目描述

给定一棵 $n$ 个结点的树，树没有边权。请求出树的直径是多少，即树上的最长路径长度是多少。

## 输入格式

第一行输入一个正整数 $n$，表示结点个数。

第二行开始，往下一共 $n-1$ 行，每一行两个正整数 $(u,v)$，表示一条边。

## 输出格式

输出一行，表示树的直径是多少。

## 样例 #1

### 样例输入 #1

```
5
1 2
2 4
4 5
2 3
```

### 样例输出 #1

```
3
```

## 提示

数据保证，$1 \leq n \leq 10^5$。

# 题解

## 树形DP

> 树形DP可以再存在负边权的情况下求解出树的直径



idx边的编号

`h` 表头数组,存储顶点i的出边的编号idx

`to` 终点数组,顶点i的邻点,idx号边的终点

`w` 边权数组,idx号边的权值

`ne` idx号边的下一条边

#### 链式前向星

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<cstring>
using namespace std;
const int MAXSIZE=1e5+5;
int to[MAXSIZE],w[MAXSIZE],ne[MAXSIZE],h[MAXSIZE],idx;
bool vis[MAXSIZE];
int ans;
void add(int a,int b,int c){//邻接表
    to[idx]=b;
    w[idx]=c;
    ne[idx]=h[a];
    h[a]=idx++;
}
int dfs(int u){
    vis[u]=true;
    int d1=0,d2=0;
    for(int i=h[u];i!=-1;i=ne[i]){
        int j=to[i];//u的邻点
        if(vis[j])
            continue;
        int dis=dfs(j)+w[i];
        if(dis>=d1){
            d2=d1;
            d1=dis;
        }
        else if(dis>d2)
            d2=dis;
    }
    ans=max(ans,d1+d2);
    return d1;
}
int main(){
    //树形DP
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    memset(h,-1,sizeof(h));
    int n,m;
    cin>>n>>m;
    int u,v,worth;
    char c;
    for(int i=1;i<=m;i++){
        cin>>u>>v>>worth>>c;
        add(u,v,worth);
        add(v,u,worth);
    }
    dfs(1);
    cout<<ans;
    return 0;
}

```

#### vector结构体

```c++
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;
const int MAXSIZE = 1e4 + 5;
struct edge {
    int v, w;
};
vector<edge> e[MAXSIZE];
bool vis[MAXSIZE];//记录点是否被搜过
int ans;

int DFS(int u) {
    vis[u] = true;//搜过的点打上标记
    int d1 = 0, d2 = 0;
    //d1记录u点往下最大长度d2记录u点往下第二长的长度
    for (auto ed: e[u]) {//枚举u的所有邻点
        int v = ed.v, w = ed.w;
        if (vis[v])//u也是v的邻点,避免向上搜卡住循环
            continue;
        int dis = DFS(v) + w;//搜到底求最大距离
        if (dis >= d1) {//更新最长和次长距离
            d2 = d1;
            d1 = dis;
        } else if (dis > d2)//小于d1且大于d2
            d2 = dis;
    }
    ans = max(ans, d1 + d2);
    return d1;//返回往下走的最大距离
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin >> n;
    for (int i = 1; i <= n - 1; i++) {
        int u, v;
        cin >> u >> v;
        //建立无向边,权值为1
        e[u].push_back({v, 1});
        e[v].push_back({u, 1});
    }
    DFS(1);//任取一个点都可以
    cout << ans;
    return 0;
}
```

## 两次DFS

首先从任意节点$y$开始进行第一次DFS，到达距离其最远的结点，记为$z$，然后再从$z$开始做第二次DFS，到达距离$z$最远的结点，记为$z'$，则$dis(z,z')$即为树的直径

定理：在一棵树上，从任意节点$y$开始进行一次DFS，到达的距离其最远的节点$z$必为直径的一端

**如果存在负边权，上述定理不成立，所以不能用两次DFS的方式求直径**

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

struct Tree {
    int n;
    vector<vector<int>> ver;
    Tree(int n) {
        this->n = n;
        ver.resize(n + 1);
    }
    void addEdge(int x, int y) {
        ver[x].push_back(y);
        ver[y].push_back(x);
    }
    int getlen(int root) { // 获取x所在树的直径
        map<int, int> dep; // map用于优化输入为森林时的深度计算，亦可用vector
        function<void(int, int)> dfs = [&](int x, int fa) -> void {
            for (auto y : ver[x]) {
                if (y == fa)
                    continue;
                dep[y] = dep[x] + 1;
                dfs(y, x);
            }
            if (dep[x] > dep[root]) {
                root = x;
            }
        };
        dfs(root, 0);
        int st = root; // 记录直径端点

        dep.clear();
        dfs(root, 0);
        int ed = root; // 记录直径另一端点

        return dep[root];
    }
};

void solve() {
    int n;
    cin >> n;
    Tree tree(n + 1);
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        tree.addEdge(u, v);
    }
    cout << tree.getlen(1) << "\n";
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

