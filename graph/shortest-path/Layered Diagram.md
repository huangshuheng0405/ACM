# 分层图最短路

# [JLOI2011] 飞行路线

## 题目描述

Alice 和 Bob 现在要乘飞机旅行，他们选择了一家相对便宜的航空公司。该航空公司一共在 $n$ 个城市设有业务，设这些城市分别标记为 $0$ 到 $n-1$，一共有 $m$ 种航线，每种航线连接两个城市，并且航线有一定的价格。

Alice 和 Bob 现在要从一个城市沿着航线到达另一个城市，途中可以进行转机。航空公司对他们这次旅行也推出优惠，他们可以免费在最多 $k$ 种航线上搭乘飞机。那么 Alice 和 Bob 这次出行最少花费多少？

## 输入格式

第一行三个整数 $n,m,k$，分别表示城市数，航线数和免费乘坐次数。

接下来一行两个整数 $s,t$，分别表示他们出行的起点城市编号和终点城市编号。

接下来 $m$ 行，每行三个整数 $a,b,c$，表示存在一种航线，能从城市 $a$ 到达城市 $b$，或从城市 $b$ 到达城市 $a$，价格为 $c$。

## 输出格式

输出一行一个整数，为最少花费。

## 样例 #1

### 样例输入 #1

```
5 6 1
0 4
0 1 5
1 2 5
2 3 5
3 4 5
2 3 3
0 2 100
```

### 样例输出 #1

```
8
```

## 提示

#### 数据规模与约定

对于 $30\%$ 的数据，$2 \le n \le 50$，$1 \le m \le 300$，$k=0$。

对于 $50\%$ 的数据，$2 \le n \le 600$，$1 \le m \le 6\times10^3$，$0 \le k \le 1$。

对于 $100\%$ 的数据，$2 \le n \le 10^4$，$1 \le m \le 5\times 10^4$，$0 \le k \le 10$，$0\le s,t,a,b < n$，$a\ne b$，$0\le c\le 10^3$。

另外存在一组 hack 数据。

# 题解

数组开大一点

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

vector<array<int, 2>> adj[200005];
int dis[200005];
bool vis[200005];

void solve() {
    int n, m, k;
    cin >> n >> m >> k;

    int s, t;
    cin >> s >> t;

    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;

        adj[u].push_back({v, w});//原图建边
        adj[v].push_back({u, w});

        for (int j = 1; j <= k; j++) {//建k层图
            adj[u + (j - 1) * n].push_back({v + j * n, 0});//在第j-1层建边权为0的边到j层
            adj[v + (j - 1) * n].push_back({u + j * n, 0});

            adj[u + j * n].push_back({v + j * n, w});//在第j层正常建边权为w的边
            adj[v + j * n].push_back({u + j * n, w});
        }
    }

    for (int i = 1; i <= k; i++) {//将每一层的终点连起来，有可能提前到终点
        adj[t + (i - 1) * n].push_back({t + i * n, 0});
    }

    memset(dis, 0x3f3f, sizeof(dis));
    dis[s] = 0;
    priority_queue<array<int, 2>> pq;
    pq.push({0, s});
    while (!pq.empty()) {
        auto [dist, u] = pq.top();
        pq.pop();
        if (vis[u]) {
            continue;
        }
        vis[u] = true;
        for (auto [v, w] : adj[u]) {
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                pq.push({-dis[v], v});
            }
        }
    }

    cout << dis[t + k * n] << "\n";//求出在第k层终点的距离
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

# [USACO09FEB] Revamping Trails G

## 题面翻译

约翰一共有 $N$ 个牧场.由 $M$ 条布满尘埃的小径连接。小径可以双向通行。每天早上约翰从牧场 $1$ 出发到牧场 $N$ 去给奶牛检查身体。

通过每条小径都需要消耗一定的时间。约翰打算升级其中 $K$ 条小径，使之成为高速公路。在高速公路上的通行几乎是瞬间完成的，所以高速公路的通行时间为 $0$。

请帮助约翰决定对哪些小径进行升级，使他每天从 $1$ 号牧场到第 $N$ 号牧场所花的时间最短。输出这一最短时间即可。

## 题目描述

Farmer John dutifully checks on the cows every day. He traverses some of the M (1 <= M <= 50,000) trails conveniently numbered 1..M from pasture 1 all the way out to pasture N (a journey which is always possible for trail maps given in the test data). The N (1 <= N <= 10,000) pastures conveniently numbered 1..N on Farmer John's farm are currently connected by bidirectional dirt trails.  Each trail i connects pastures P1\_i and P2\_i (1 <= P1\_i <= N; 1 <= P2\_i <= N) and requires T\_i (1 <= T\_i <= 1,000,000) units of time to traverse.

He wants to revamp some of the trails on his farm to save time on his long journey. Specifically, he will choose K (1 <= K <= 20) trails to turn into highways, which will effectively reduce the trail's traversal time to 0. Help FJ decide which trails to revamp to minimize the resulting time of getting from pasture 1 to N.

TIME LIMIT: 2 seconds

## 输入格式

\* Line 1: Three space-separated integers: N, M, and K

\* Lines 2..M+1: Line i+1 describes trail i with three space-separated integers: P1\_i, P2\_i, and T\_i

## 输出格式

\* Line 1: The length of the shortest path after revamping no more than K edges

## 样例 #1

### 样例输入 #1

```
4 4 1 
1 2 10 
2 4 10 
1 3 1 
3 4 100
```

### 样例输出 #1

```
1
```

## 提示

K is 1; revamp trail 3->4 to take time 0 instead of 100. The new shortest path is 1->3->4, total traversal time now 1.

# 题解

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

vector<array<i64, 2>> adj[500005];
i64 dis[500005];
bool vis[500005];

void solve() {
    int n, m, k;
    cin >> n >> m >> k;

    int s = 1, t = n;

    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;

        adj[u].push_back({v, w}); // 原图建边
        adj[v].push_back({u, w});

        for (int j = 1; j <= k; j++) { // 向下建k层图
            adj[u + (j - 1) * n].push_back({v + j * n, 0}); // 在第j-1层建边权为0的边到j层
            adj[v + (j - 1) * n].push_back({u + j * n, 0});

            adj[u + j * n].push_back({v + j * n, w}); // 在第j层正常建边权为w的边
            adj[v + j * n].push_back({u + j * n, w});
        }
    }

    for (int i = 1; i <= k; i++) { // 将每一层的重点连起来，有可能提前到终点
        adj[t + (i - 1) * n].push_back({t + i * n, 0});
    }

    memset(dis, 0x3f3f3f3f, sizeof(dis));
    dis[s] = 0;
    priority_queue<array<i64, 2>> pq;
    pq.push({0, s});
    while (!pq.empty()) {
        auto [dist, u] = pq.top();
        pq.pop();
        if (vis[u]) {
            continue;
        }
        vis[u] = true;
        for (auto [v, w] : adj[u]) {
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                pq.push({-dis[v], v});
            }
        }
    }

    cout << dis[t + k * n] << "\n"; // 求出在第k层终点的距离
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

