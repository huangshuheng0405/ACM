# SPFA

#### Shortest Path Faster Algorithm

可以求出有负边权的图的最短路并可以对**最短路不存在**的情况进行判断

以一个点出发不能抵达负环，不能说明图上不存在负环，要判断整个图上是否存在负环，向图上每个节点连一条权值为零的边，开始执行SPFA

对于边$(u,v)$，松弛操作对应下面的式子：$\large dis(v)=min(dis(v),dis(u)+w(u,v))$



# 【模板】负环

## 题目描述

给定一个 $n$ 个点的有向图，请求出图中是否存在**从顶点 $1$ 出发能到达**的负环。

负环的定义是：一条边权之和为负数的回路。

## 输入格式

**本题单测试点有多组测试数据**。

输入的第一行是一个整数 $T$，表示测试数据的组数。对于每组数据的格式如下：

第一行有两个整数，分别表示图的点数 $n$ 和接下来给出边信息的条数 $m$。

接下来 $m$ 行，每行三个整数 $u, v, w$。

- 若 $w \geq 0$，则表示存在一条从 $u$ 至 $v$ 边权为 $w$ 的边，还存在一条从 $v$ 至 $u$ 边权为 $w$ 的边。
- 若 $w < 0$，则只表示存在一条从 $u$ 至 $v$ 边权为 $w$ 的边。

## 输出格式

对于每组数据，输出一行一个字符串，若所求负环存在，则输出 `YES`，否则输出 `NO`。

## 样例 #1

### 样例输入 #1

```
2
3 4
1 2 2
1 3 4
2 3 1
3 1 -3
3 3
1 2 3
2 3 4
3 1 -8
```

### 样例输出 #1

```
NO
YES
```

## 提示

#### 数据规模与约定

对于全部的测试点，保证：

- $1 \leq n \leq 2 \times 10^3$，$1 \leq m \leq 3 \times 10^3$。
- $1 \leq u, v \leq n$，$-10^4 \leq w \leq 10^4$。
- $1 \leq T \leq 10$。

#### 提示

请注意，$m$ **不是**图的边数。

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve()
{
    int n, m;
    cin >> n >> m;
    vector<pair<int, int>> adj[n + 1];
    for (int i = 1; i <= m; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
        if (w >= 0)
        {
            adj[v].push_back({u, w});
        }
    }
    vector<bool> vis(n + 1);
    vector<int> dis(n + 1, 1e9), cnt(n + 1);
    auto SPFA = [&]()
    {
        dis[1] = 0;
        queue<int> q;
        q.push(1);
        while (!q.empty())
        {
            auto u = q.front();
            q.pop();
            vis[u] = false;
            for (auto [v, w] : adj[u])
            {
                if (dis[v] > dis[u] + w)
                {
                    dis[v] = dis[u] + w;
                    cnt[v] = cnt[u] + 1;
                    if (cnt[v] >= n)
                    {
                        return true;
                    }
                    if (vis[v] == false)
                    {
                        q.push(v);
                        vis[v] = true;
                    }
                }
            }
        }
        return false;
    };
    if (SPFA() == true)
    {
        cout << "YES" << '\n';
    }
    else
    {
        cout << "NO" << '\n';
    }
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    cin >> _;
    while (_--)
    {
        solve();
    }
    return 0;
}
```

