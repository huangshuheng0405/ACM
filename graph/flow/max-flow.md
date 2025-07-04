# P3376 【模板】网络最大流

## 题目描述

如题，给出一个网络图，以及其源点和汇点，求出其网络最大流。

## 输入格式

第一行包含四个正整数 $n,m,s,t$，分别表示点的个数、有向边的个数、源点序号、汇点序号。

接下来 $m$ 行每行包含三个正整数 $u_i,v_i,w_i$，表示第 $i$ 条有向边从 $u_i$ 出发，到达 $v_i$，边权为 $w_i$（即该边最大流量为 $w_i$）。

## 输出格式

一行，包含一个正整数，即为该网络的最大流。

## 输入输出样例 #1

### 输入 #1

```
4 5 4 3
4 2 30
4 3 20
2 3 20
2 1 30
1 3 30
```

### 输出 #1

```
50
```

## 说明/提示

#### 样例输入输出 1 解释

 ![](https://cdn.luogu.com.cn/upload/pic/2262.png) 

题目中存在 $3$ 条路径：

- $4\to 2\to 3$，该路线可通过 $20$ 的流量。
- $4\to 3$，可通过 $20$ 的流量。
- $4\to 2\to 1\to 3$，可通过 $10$ 的流量（边 $4\to 2$ 之前已经耗费了 $20$ 的流量）。

故流量总计 $20+20+10=50$。输出 $50$。

---

#### 数据规模与约定

- 对于 $30\%$ 的数据，保证 $n\leq10$，$m\leq25$。
- 对于 $100\%$ 的数据，保证 $1 \leq n\leq200$，$1 \leq m\leq 5000$，$0 \leq w\lt 2^{31}$。

# 题解

`mf`存$S~v$的路径上的流量上限

`pre[v]`存$v$的前驱边

#### $bfs$​找增广路

1. 初始化，$mf[]=0,mf[S]=\infty,S$入队

2. 只要队不空，$u$点出队，

   （1）枚举$u$的所有出边，更新$u$的流量上限，记录前驱边，扩展儿子入队

   （2）若能走到$T$点，则返回$true$

   （3）若不能走到$T$点，返回$false$

#### $EK$求最大流

循环找增广路，每找到一条

1. 逆序更新残留网，容量此消彼长
2. 累加可行流，最后返回最大流

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;
const int N = 1E4 + 5;
struct Edge {
    i64 v, c, ne;
} e[200005];
int h[N], cur = 1;
i64 mf[N], pre[N];

void add(int a, int b, int c) {
    e[++cur] = {b, c, h[a]};
    h[a] = cur;
}

void solve() {
    int n, m, S, T;
    cin >> n >> m >> S >> T;
    auto bfs = [&]() -> bool {
        memset(mf, 0, sizeof(mf));
        queue<int> q;
        q.push(S);
        mf[S] = 1E9;
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            for (int i = h[u]; i; i = e[i].ne) {
                i64 v = e[i].v;
                if (mf[v] == 0 && e[i].c) {
                    mf[v] = min(mf[u], e[i].c); // 更新流量上限
                    pre[v] = i;                 // 存前驱边
                    q.push(v);
                    if (v == T) {
                        return true;
                    }
                }
            }
        }
        return false;
    };
    auto EK = [&]() -> i64 {
        i64 flow = 0;
        while (bfs()) {
            int v = T;
            while (v != S) {
                int i = pre[v];
                e[i].c -= mf[T];
                e[i ^ 1].c += mf[T];
                v = e[i ^ 1].v;
            }
            flow += mf[T];
        }
        return flow;
    };
    while (m--) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
        add(b, a, 0);
    }
    cout << EK() << "\n";
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

# 板子

```c++
#include <bits/stdc++.h>

using i64 = long long;

constexpr int inf = 1E9;
template <class T> struct MaxFlow {
    struct _Edge {
        int to;
        T cap;
        _Edge(int to, T cap) : to(to), cap(cap) {}
    };

    int n;
    std::vector<_Edge> e;
    std::vector<std::vector<int>> g;
    std::vector<int> cur, h;

    MaxFlow() {}
    MaxFlow(int n) { init(n); }

    void init(int n) {
        this->n = n;
        e.clear();
        g.assign(n, {});
        cur.resize(n);
        h.resize(n);
    }

    bool bfs(int s, int t) {
        h.assign(n, -1);
        std::queue<int> que;
        h[s] = 0;
        que.push(s);
        while (!que.empty()) {
            const int u = que.front();
            que.pop();
            for (int i : g[u]) {
                auto [v, c] = e[i];
                if (c > 0 && h[v] == -1) {
                    h[v] = h[u] + 1;
                    if (v == t) {
                        return true;
                    }
                    que.push(v);
                }
            }
        }
        return false;
    }

    T dfs(int u, int t, T f) {
        if (u == t) {
            return f;
        }
        auto r = f;
        for (int &i = cur[u]; i < int(g[u].size()); ++i) {
            const int j = g[u][i];
            auto [v, c] = e[j];
            if (c > 0 && h[v] == h[u] + 1) {
                auto a = dfs(v, t, std::min(r, c));
                e[j].cap -= a;
                e[j ^ 1].cap += a;
                r -= a;
                if (r == 0) {
                    return f;
                }
            }
        }
        return f - r;
    }
    void addEdge(int u, int v, T c) {
        g[u].push_back(e.size());
        e.emplace_back(v, c);
        g[v].push_back(e.size());
        e.emplace_back(u, 0);
    }
    T flow(int s, int t) {
        T ans = 0;
        while (bfs(s, t)) {
            cur.assign(n, 0);
            ans += dfs(s, t, std::numeric_limits<T>::max());
        }
        return ans;
    }

    std::vector<bool> minCut() {
        std::vector<bool> c(n);
        for (int i = 0; i < n; i++) {
            c[i] = (h[i] != -1);
        }
        return c;
    }

    struct Edge {
        int from;
        int to;
        T cap;
        T flow;
    };
    std::vector<Edge> edges() {
        std::vector<Edge> a;
        for (int i = 0; i < e.size(); i += 2) {
            Edge x;
            x.from = e[i + 1].to;
            x.to = e[i].to;
            x.cap = e[i].cap + e[i + 1].cap;
            x.flow = e[i + 1].cap;
            a.push_back(x);
        }
        return a;
    }
};

void solve() {
    int n, m, S, T;
    std::cin >> n >> m >> S >> T;

    MaxFlow<i64> mf(n + 1);

    while (m--) {
        int u, v, w;
        std::cin >> u >> v >> w;

        mf.addEdge(u, v, w);
        mf.addEdge(v, u, 0);
    }

    std::cout << mf.flow(S, T) << "\n";
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

