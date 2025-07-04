# 最小生成树(Prim)

Prim 算法是另一种常见并且好写的最小生成树算法。该算法的基本思想是从一个结点开始，不断加点（而不是 Kruskal 算法的加边）。

具体来说，每次要选择距离最小的一个结点，以及用新的边更新其他结点的距离。

其实跟 Dijkstra 算法一样，每次找到距离最小的一个点，可以暴力找也可以用堆维护。

堆优化的方式类似 Dijkstra 的堆优化，但如果使用二叉堆等不支持$O(1)$decrease-key 的堆，复杂度就不优于 Kruskal，常数也比 Kruskal 大。所以，一般情况下都使用 Kruskal 算法，在稠密图尤其是完全图上，暴力 Prim 的复杂度比 Kruskal 优，但 **不一定** 实际跑得更快。

# 【模板】最小生成树

## 题目描述

如题，给出一个无向图，求出最小生成树，如果该图不连通，则输出 `orz`。

## 输入格式

第一行包含两个整数 $N,M$，表示该图共有 $N$ 个结点和 $M$ 条无向边。

接下来 $M$ 行每行包含三个整数 $X_i,Y_i,Z_i$，表示有一条长度为 $Z_i$ 的无向边连接结点 $X_i,Y_i$。

## 输出格式

如果该图连通，则输出一个整数表示最小生成树的各边的长度之和。如果该图不连通则输出 `orz`。

## 样例 #1

### 样例输入 #1

```
4 5
1 2 2
1 3 2
1 4 3
2 3 4
3 4 3
```

### 样例输出 #1

```
7
```

## 提示

数据规模：

对于 $20\%$ 的数据，$N\le 5$，$M\le 20$。

对于 $40\%$ 的数据，$N\le 50$，$M\le 2500$。

对于 $70\%$ 的数据，$N\le 500$，$M\le 10^4$。

对于 $100\%$ 的数据：$1\le N\le 5000$，$1\le M\le 2\times 10^5$，$1\le Z_i \le 10^4$。


样例解释：

 ![](https://cdn.luogu.com.cn/upload/pic/2259.png) 

所以最小生成树的总边权为 $2+2+3=7$。

# solve

priority_queue（堆优化)

```c++
#include <bits/stdc++.h>

using i64 = long long;

struct Edge {
    int v, w;
};

int main() {
    // 最小生成树,用最小的距离把所有点找到
    // 每次选最小的距离
    int n, m;
    std::cin >> n >> m;
    std::vector<Edge> adj[n + 1];
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        std::cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }
    i64 ans = 0;
    int cnt = 0;
    auto MST = [&]() -> bool {
        std::vector<int> dis(n + 1, 1E9);
        std::vector<bool> vis(n + 1);
        dis[1] = 0;
        std::priority_queue<std::pair<int, int>> q;
        q.push({0, 1});
        while (!q.empty()) {
            auto t = q.top();
            q.pop();
            int u = t.second;
            if (vis[u])
                continue;
            vis[u] = true; // 搜过的点打上标记
            ans += dis[u];
            cnt++;
            for (auto [v, w] : adj[u]) {
                if (dis[v] > w) {
                    dis[v] = w;
                    q.push({-dis[v], v});
                }
            }
        }
        return cnt == n; // 判断是否有最小生成树（出队的点够不够所有点）
    };
    if (MST())
        std::cout << ans << "\n";
    else
        std::cout << "orz";
    return 0;
}
```

## 类似Dijkstra，不断选距离最小的点出圈，知道圈内为空

#### 1.初始化，所有点在圈内vis=0，dis[s]=0,其他店都为无穷大

#### 2.每次从圈内选取一个距离最小的点u，打标记把他移出圈

#### 3.对u的所有邻点的距离进行更新操作

#### 4.重复2、3步骤，知道圈内为空

###### 没有堆优化版的prim

```c++
const int N = 2e5 + 5, mod = 1e9 + 7, inf = 0x3f3f3f3f;
int T, ans, dis[N], n, cnt;
struct edge {
    int v, w;
};
vector<edge> e[N];
bool vis[N];

bool prim(int s) {
    for (int i = 1; i <= n; i++)
        dis[i] = inf;
    dis[s] = 0;
    for (int i = 1; i <= n; i++) {
        int u = 0;
        for (int j = 1; j <= n; j++)//每次找出圈内最小的数
            if (!vis[j] && dis[j] < dis[u])
                u = j;
        vis[u] = 1;
        ans += dis[u];
        if (dis[u] != inf)//有些图是不连通的
            cnt++;
        for (auto j: e[u]) {//对u的邻点更新距离
            int v = j.v, w = j.w;
            if (dis[v] > w)
                dis[v] = w;
        }
    }
    return cnt == n;
}
```

```c++
// Problem: 最短网络
// Contest: AcWing
// URL: https://www.acwing.com/problem/content/1142/
// Memory Limit: 64 MB
// Time Limit: 1000 ms
//
// Powered by CP Editor (https://cpeditor.org)

#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <string>
#include <vector>
using namespace std;
#define ll long long
#define ull unsigned long long
#define db double
#define lb long double
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
// int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左

void solve() {
  int n;
  cin >> n;
  vector<pair<int, int>> adj[n + 1];
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      int x;
      cin >> x;
      adj[i].push_back({j, x});
    }
  }
  ll ans = 0;
  vector<int> dis(n + 1, 1e9);
  vector<bool> vis(n + 1);
  dis[1] = 0;
  priority_queue<pair<int, int>> q;
  q.push({0, 1});
  auto prim = [&]() {
    while (!q.empty()) {
      auto t = q.top();
      q.pop();
      int u = t.second;
      if (vis[u]) {
        continue;
      }
      vis[u] = true;
      ans += dis[u];
      for (auto i : adj[u]) {
        auto [v, w] = i;
        if (dis[v] > w) {
          dis[v] = w;
          q.push({-dis[v], v});
        }
      }
    }
  };
  prim();
  cout << ans << endl;
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

