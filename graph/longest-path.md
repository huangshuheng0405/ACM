

# 最长路

## 题目描述

设 $G$ 为有 $n$ 个顶点的带权有向无环图，$G$ 中各顶点的编号为 $1$ 到 $n$，请设计算法，计算图 $G$ 中 $1, n$ 间的最长路径。

## 输入格式

输入的第一行有两个整数，分别代表图的点数 $n$ 和边数 $m$。

第 $2$ 到第 $(m + 1)$ 行，每行 $3$ 个整数 $u, v, w$（$u<v$），代表存在一条从 $u$ 到  $v$ 边权为 $w$ 的边。

## 输出格式

输出一行一个整数，代表 $1$ 到 $n$ 的最长路。

若 $1$ 无法到达 $n$，请输出 $-1$。

## 样例 #1

### 样例输入 #1

```
2 1
1 2 1
```

### 样例输出 #1

```
1
```

## 提示

**【数据规模与约定】**

- 对于 $20\%$的数据，$n \leq 100$，$m \leq 10^3$。
- 对于 $40\%$ 的数据，$n \leq 10^3$，$m \leq 10^{4}$。
- 对于 $100\%$ 的数据，$1 \leq n \leq 1500$，$0 \leq m \leq 5 \times 10^4$，$1 \leq u, v \leq n$，$-10^5 \leq w \leq 10^5$。

# 题解

## 拓扑排序+DP

一定要是向无环图

#### 拓扑排序后，按照拓扑排序遍历每个节点，用当前节点来更新之后的节点。

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
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
int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//上右下左

vector<int> pre(1e5);

void solve() {
    int n, m;
    cin >> n >> m;
    vector<pair<ll, ll>> adj[n + 5];
    vector<int> ru(n + 5);
    for (int i = 1; i <= m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        adj[a].push_back({b, c});
        ru[b]++;
    }
    queue<int> q;
    for (int i = 1; i <= n; i++) {
        if (ru[i] == 0) {
            q.push(i);
        }
    }
    vector<int> ans;//拓扑排序后的序列
    vector<ll> dis(n + 5, -1e18);
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        ans.push_back(u);
        for (int i = 0; i < adj[u].size(); i++) {
            auto [v, w] = adj[u][i];
            if (--ru[v] == 0) {
                q.push(v);
            }
        }
    }
    dis[1] = 0;
    for (int i = 0; i < ans.size(); i++) {
        int u = ans[i];
        for (int j = 0; j < adj[u].size(); j++) {
            auto [v, w] = adj[u][j];
            if (dis[v] < dis[u] + w) {//更新最大距离
                dis[v] = dis[u] + w;
                pre[v] = u;
            }
        }
    }
    if (dis[n] == -1e18) {//到达不了
        cout << -1;
    } else {
        cout << dis[n];
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# 打印路径

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
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
int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//上右下左

vector<int>pre(1e5+5);

void path(int x){
    if(x==1){
        cout<<1<<' ';
        return;
    }
    path(pre[x]);
    cout<<x<<' ';
}

void solve() {
    int n,m;
    cin>>n>>m;
    vector<int>adj[n+5];
    vector<int>ru(n+5,0);
    for(int i=1;i<=m;i++){
        int a,b;
        cin>>a>>b;
        adj[a].push_back(b);
        ru[b]++;
    }
    queue<int>q;
    vector<int>tp(n+5);
    for(int i=1;i<=n;i++){
        if(ru[i]==0){
            q.push(i);
        }
    }
    while(!q.empty()){
        int u=q.front();
        q.pop();
        tp.push_back(u);
        for(int i=0;i<adj[u].size();i++){
            int v=adj[u][i];
            if(--ru[v]==0){
                q.push(v);
            }
        }
    }
    vector<ll>dis(n+5,-1e18);
    dis[1]=1;
    for(int i=0;i<tp.size();i++){
        int u=tp[i];
        for(int j=0;j<adj[u].size();j++){
            int v=adj[u][j];
            if(dis[v]<dis[u]+1){
                dis[v]=dis[u]+1;
                pre[v]=u;
            }
        }
    }
    if(dis[n]!=-1e18){
        cout<<dis[n]<<endl;
        path(n);//打印路径
    }else{//到不了终点
        cout<<"IMPOSSIBLE";
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--)
        solve();
    return 0;
}
```

