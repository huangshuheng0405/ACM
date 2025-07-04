# 最小环

# 无向图的最小环问题

## 题目描述

给定一张无向图，求图中一个至少包含 $3$ 个点的环，环上的节点不重复，并且环上的边的长度之和最小。该问题称为无向图的最小环问题。在本题中，你需要输出最小的环的边权和。若无解，输出 `No solution.`。

## 输入格式

第一行两个正整数 $n,m$ 表示点数和边数。

接下来 $m$ 行，每行三个正整数 $u,v,d$，表示节点 $u,v$ 之间有一条长度为 $d$ 的边。

## 输出格式

输出边权和最小的环的边权和。若无解，输出 `No solution.`。

## 样例 #1

### 样例输入 #1

```
5 7
1 4 1
1 3 300
3 1 10
1 2 16
2 3 100
2 5 15
5 3 20
```

### 样例输出 #1

```
61
```

## 提示

样例解释：一种可行的方案：$1-3-5-2-1$。

对于 $20\%$ 的数据，$n,m \leq 10$。

对于 $60\%$ 的数据，$m\leq 100$。

对于 $100\%$ 的数据，$1\le n\leq 100$，$1\le m\leq 5\times 10^3$，$1 \leq d \leq 10^5$。

**无解输出包括句号。**

# 题解

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
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//上右下左

ll dp[110][110], dis[110][110];

void solve() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i != j) {
                dp[i][j] = inf;
            }
        }
    }
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        dp[u][v] = dp[v][u] = w;
    }
    ll ans = inf;
    memcpy(dis, dp, sizeof(dis));// 将dp的内容赋值给dis数组
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i < k; i++) {
            for (int j = i + 1; j < k; j++) {
                ans = min(ans, dis[i][j] + dp[j][k] + dp[k][i]);// 找最小环
            }
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);// 更新最短路
            }
        }
    }
    if (ans == inf) {
        cout << "No solution.";
    } else {
        cout << ans;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;
    //cin >> _;
    while (_--)
        solve();
    return 0;
}
```

