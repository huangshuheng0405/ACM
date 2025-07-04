# 最小生成树(Kruskal)

[MST](https://oi-wiki.org/graph/mst/#kruskal-%E7%AE%97%E6%B3%95)

前置知识：并查集、贪心、图的存储

从最小边权开始，按边权从小到大依次加入（贪心），利用**并查集**来维护是否产生了环，如果某次加边产生了环，就不加这条边，直到加入了$n-1$条边，就形成了一棵树

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



```c++
#include <bits/stdc++.h>

using i64 = long long;

struct Edge {
    int u, v, w;
    bool operator<(const Edge &t) const { 
    	return w < t.w; 
    }
};

void solve() {
    int n, m;
    std::cin >> n >> m;
    std::vector<int> fa(n + 1);
    std::iota(fa.begin(), fa.end(), 0);
    std::vector<Edge> adj(m);
    for (int i = 0; i < m; i++) {
        int x, y, z;
        std::cin >> x >> y >> z;
        adj[i] = {x, y, z};
    }
    std::sort(adj.begin(), adj.end());
    auto find = [&](auto &&self, int x) -> int {
        return x == fa[x] ? x : fa[x] = self(self, fa[x]);
    };
    i64 ans = 0, cnt = 0;
    for (int i = 0; i < m; i++) {
        auto [u, v, w] = adj[i];
        int x = find(find, u), y = find(find, v);
        if (x != y) {
            fa[x] = y;
            ans += w;
            cnt++;
        }
    }
    if (cnt == n - 1) {
        std::cout << ans << "\n";
    } else {
        std::cout << "orz\n";
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```
