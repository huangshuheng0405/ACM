# Dijkstra

求解**非负权图**上单源最短路径的算法

dijksta不能用于带负边权的图和判断是否有负环

可以判断大一点的图

dijkstra不用考虑重边，反正会走最小的路

## 思路

1.每次从未标记的节点中选择离出发点最近的节点，标记，记录到最优路径集合中

2.计算刚加入节点A的邻近节点B的距离（不包含标记的节点），若（节点A的距离+节点A到节点B的边长）<节点B的距离，则更新节点B的距离和前面点

# P4779 【模板】单源最短路径（标准版）

## 题目背景

2018 年 7 月 19 日，某位同学在 [NOI Day 1 T1 归程](https://www.luogu.org/problemnew/show/P4768) 一题里非常熟练地使用了一个广为人知的算法求最短路。

然后呢？

$100 \rightarrow 60$；

$\text{Ag} \rightarrow \text{Cu}$；

最终，他因此没能与理想的大学达成契约。

小 F 衷心祝愿大家不再重蹈覆辙。

## 题目描述

给定一个 $n$ 个点，$m$ 条有向边的带非负权图，请你计算从 $s$ 出发，到每个点的距离。

数据保证你能从 $s$ 出发到任意点。

## 输入格式

第一行为三个正整数 $n, m, s$。
第二行起 $m$ 行，每行三个非负整数 $u_i, v_i, w_i$，表示从 $u_i$ 到 $v_i$ 有一条权值为 $w_i$ 的有向边。

## 输出格式

输出一行 $n$ 个空格分隔的非负整数，表示 $s$ 到每个点的距离。

## 输入输出样例 #1

### 输入 #1

```
4 6 1
1 2 2
2 3 2
2 4 1
1 3 5
3 4 3
1 4 4
```

### 输出 #1

```
0 2 4 3
```

## 说明/提示

样例解释请参考 [数据随机的模板题](https://www.luogu.org/problemnew/show/P3371)。

$1 \leq n \leq 10^5$；

$1 \leq m \leq 2\times 10^5$；

$s = 1$；

$1 \leq u_i, v_i\leq n$；

$0 \leq w_i \leq 10 ^ 9$,

$0 \leq \sum w_i \leq 10 ^ 9$。

本题数据可能会持续更新，但不会重测，望周知。

2018.09.04 数据更新 from @zzq

图的点数超过10^5，没优化的dijkstra就会超时

# Solve

将结点分成两个集合：已确定最短路长度的点集（记为 $S$ 集合）的和未确定最短路长度的点集（记为$T$集合）。一开始所有的点都属于$T$集合。

初始化$dis(s)=0$，其他点的$dis$均为$+\infty$。

然后重复这些操作：

1. 从$T$集合中，选取一个最短路长度最小的结点，移到$S$集合中。
2. 对那些刚刚被加入$S$集合的结点的所有出边执行松弛操作。

直到$T$集合为空，算法结束。

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n, m, s;
    std::cin >> n >> m >> s;

    std::vector<std::vector<std::array<int, 2>>> adj(n + 1);
    for (int i = 0; i < m; i++) {
        int u, v, w;
        std::cin >> u >> v >> w;

        adj[u].push_back({v, w});
    }

    std::vector<int> dis(n + 1, 1E9);
    dis[s] = 0;
    std::priority_queue<std::array<int, 2>> pq;
    pq.push({0, s});
    std::vector<bool> vis(n + 1);
    while (!pq.empty()) {
        auto [x, y] = pq.top();
        pq.pop();
        if (vis[y]) {
            continue;
        }
        vis[y] = 1;
        for (auto [v, w] : adj[y]) {
            if (dis[v] > dis[y] + w) {
                dis[v] = dis[y] + w;
                pq.push({-dis[v], v});
            }
        }
    }

    for (int i = 1; i <= n; i++) {
        std::cout << dis[i] << " ";
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);
    int t = 1;

    // std::cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

# 输出路径

开一个`p`在更新距离的时候记录下来后面的点是由哪个点转移过去的，算法结束后再递归地输出路径即可。

Floyed是记录`pre[i][j]=k`;  Bellman_Ford和Dijkstra一般记录`pre[v]=u`