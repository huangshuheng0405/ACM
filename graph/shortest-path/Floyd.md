# Floyd(全源最短路)

可以求出任意两个点之间的最短距离

复杂度比较高,n=100,有向无向，边权正负都可以,**不能有负环**

# 【模板】Floyd

## 题目描述

给出一张由 $n$ 个点 $m$ 条边组成的无向图。

求出所有点对 $(i,j)$ 之间的最短路径。

## 输入格式

第一行为两个整数 $n,m$，分别代表点的个数和边的条数。

接下来 $m$ 行，每行三个整数 $u,v,w$，代表 $u,v$ 之间存在一条边权为 $w$ 的边。

## 输出格式

输出 $n$ 行每行 $n$ 个整数。

第 $i$ 行的第 $j$ 个整数代表从 $i$ 到 $j$ 的最短路径。

## 样例 #1

### 样例输入 #1

```
4 4
1 2 1
2 3 1
3 4 1
4 1 1
```

### 样例输出 #1

```
0 1 2 1
1 0 1 2
2 1 0 1
1 2 1 0
```

## 提示

对于 $100\%$ 的数据，$n \le 100$，$m \le 4500$，任意一条边的权值 $w$ 是正整数且 $1 \leqslant w \leqslant 1000$。

**数据中可能存在重边。**

# 题解

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve()
{
    int n, m;
    cin >> n >> m;
    vector<vector<int>> dis(n + 1, vector<int>(n + 1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            i == j ? dis[i][j] = 0 : dis[i][j] = 1e9;
        }
    }
    for (int i = 1; i <= m; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        dis[u][v] = min(dis[u][v], w);//可能重边取最小边
        dis[v][u] = min(dis[v][u], w);
    }
    auto Floyd = [&]()
    {
        for (int k = 1; k <= n; k++)
        {
            for (int i = 1; i <= n; i++)
            {
                for (int j = 1; j <= n; j++)
                {
                    dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
                }
            }
        }
    };
    Floyd();
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            cout << dis[i][j] << ' ';
        }
        cout << "\n";
    }
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--)
    {
        solve();
    }
    return 0;
}
```

