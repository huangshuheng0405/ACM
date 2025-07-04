# 换根dp

树形$DP$中的换根dp问题又被称为二次扫描，通常不会指定根节点，并且根节点的变化会对一些值，例如，子节点深度和、点权和等产生影响。

通常需要两次$DFS$，第一次$DFS$预处理诸如深度，点权和之类的信息，在第二次$DFS$开始运行换根动态规划

# P3478 [POI 2008] STA-Station

## 题目描述

给定一个 $n$ 个点的树，请求出一个结点，使得以这个结点为根时，所有结点的深度之和最大。

一个结点的深度之定义为该节点到根的简单路径上边的数量。

## 输入格式

第一行有一个整数，表示树的结点个数 $n$。  
接下来 $(n - 1)$ 行，每行两个整数 $u, v$，表示存在一条连接 $u, v$ 的边。

## 输出格式

**本题存在 Special Judge**。

输出一行一个整数表示你选择的结点编号。如果有多个结点符合要求，输出任意一个即可。

## 输入输出样例 #1

### 输入 #1

```
8
1 4
5 6
4 5
6 7
6 8
2 4
3 4
```

### 输出 #1

```
7
```

## 说明/提示

#### 样例 1 解释

输出 $7$ 和 $8$ 都是正确答案。

#### 数据规模与约定

对于全部的测试点，保证 $1 \leq n \leq 10^6$，$1 \leq u, v \leq n$，给出的是一棵树。

# 题解

令$u$为当前节点，$v$为当前节点的子节点。首先需要用$siz_i$来表示以$i$为根的子树中的节点个数，并且有$siz_u=1+\sum s_v$。第一次$DFS$来计算所有的$s_i$，这次的$DFS$就是预处理，得到以某个节点为根时其子树中的节点总数

在考虑状态转移，这里体现“换根”，令$f_u$为以$u$​为根时，所有节点的深度之和。

$f_v\leftarrow f_u$可以体现换根，即以$u$为根转移到以$v$为根。显然在换根的转移过程中，以$v$为根或以$u$为根会导致其子树中的节点的深度产生改变，具体表现为：

- 所有在$v$的子树上的节点深度都减少了一，那么总深度和就减少了$siz_v$
- 右有不在$v$的子树上的节点深度都增加了一，那么总深度和就增加了$n-siz_v$

根据这两个条件就可以推出状态转移方程$f_v=f_u-siz_v+n-s_v=f_u+n-2\times siz_v$​

所以在第二次$DFS$遍历整棵树并进行状态转移,就能求出每个节点为根时的深度和

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n;
    std::cin >> n;
    std::vector<std::vector<int>> adj(n + 1);
    for (int i = 1; i < n; i++) {
        int u, v;
        std::cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    std::vector<i64> siz(n + 1), dep(n + 1), f(n + 1);
    auto dfs = [&](auto &&self, int u, int fa) -> void {//预处理
        dep[u] = dep[fa] + 1;
        siz[u] = 1;
        f[1] += dep[u];
        for (auto v : adj[u]) {
            if (v == fa) {
                continue;
            }
            self(self, v, u);
            siz[u] += siz[v];
        }
    };
    dfs(dfs, 1, 0);

    auto dfs2 = [&](auto &&self, int u, int fa) -> void {//换根dp
        for (auto v : adj[u]) {
            if (v == fa) {
                continue;
            }
            f[v] = f[u] - siz[v] + n - siz[v];
            self(self, v, u);
        }
    };
    dfs2(dfs2, 1, 0);

    i64 mx = 0, ans = 0;
    for (int i = 1; i <= n; i++) {
        if (f[i] > mx) {
            mx = f[i];
            ans = i;
        }
    }

    std::cout << ans << "\n";
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

# CF219D Choosing Capital for Treeland

## 题目描述

Treeland 国有 $n$ 个城市，有些城市间存在 **单向** 道路。这个国家一共有 $n - 1$ 条路。我们知道，如果把边视作双向的，那么从任意城市出发能到达任意城市。

城市的委员会最近决定为 Treeland 国选择一个首都，显然首都会是国中的一个城市。委员会将在首都开会，并经常取其他城市（这里不考虑从其他城市回到首都）。因此，如果城市 $a$ 被选为首都，那么所有的道路应该被定向，以使得我们能从城市 $a$ 到达其他城市。所以，有些路可能需要反转方向。

帮助委员会选择首都使得他们需要反转道路的次数最小。

## 输入格式

第一行有一个整数 $n$（$2 \le n \le 2 \times 10^5$），表示城市的数量。

接下来 $n- 1$ 行描述道路，每个道路一行，用一对整数 $s_i,t_i$（$1 \le s_i,t_i\le n$，$s_i \ne t_i$）表示该道路连接的两个城市，第 $i$ 条道路的方向是从城市 $s_i$ 到城市 $t_i$。

你可以认为 Treeland 国的城市被编号为 $1$ 到 $n$。

## 输出格式

第一行输出需要反转的道路数量的最小值。第二行输出所有可能的选择首都的方式，你需要以从小到大的顺序输出所有可能的城市编号。

Translated by uid $408071$.

## 输入输出样例 #1

### 输入 #1

```
3
2 1
2 3
```

### 输出 #1

```
0
2
```

## 输入输出样例 #2

### 输入 #2

```
4
1 4
2 4
3 4
```

### 输出 #2

```
2
1 2 3
```

# 题解

题目是单向边，存储的时候可以用双向边来存，用`bool`来表示方向（false表示正向边，true表示反向边），第一次$DFS$计算$u$点到子节点需要逆转的边数。第二次$DFS$中，$dp[i]$的定义变成u到全树节点需要逆转的边数，加入$f$是$u$的父亲，那么$f\rightarrow u$这条边是正向的时候，以$u$为首都则需要将这条边逆转，$dp[u]=dp[u]+dp[f]+1$。那么$f\rightarrow u$这条边是反向的时候，以$u$为首都不需要将这条边逆转，但由于$u$是$f$的子节点，在$dp[f]$中将这条边已经逆转过了，所以$dp[u]=dp[u]+dp[f]-1$

```c++
#include <bits/stdc++.h>

using i64 = long long;

struct Edge {
    int to, ne;
    bool w;
};

void solve() {
    int n;
    std::cin >> n;

    std::vector<Edge> e(2 * n + 1);
    std::vector<int> h(n + 1);
    int cur = 0;
    auto add = [&](int u, int v, bool w) {
        e[++cur] = {v, h[u], w};
        h[u] = cur;
    };
    for (int i = 1; i < n; i++) {
        int u, v;
        std::cin >> u >> v;
        add(u, v, false); // 正向
        add(v, u, true);  // 反向
    }

    std::vector<int> dp(n + 1);
    auto dfs1 = [&](auto &&self, int u, int fa) -> void {
        for (int i = h[u]; i; i = e[i].ne) {
            int v = e[i].to;
            if (v == fa) {
                continue;
            }
            self(self, v, u);
            dp[u] += dp[v] + e[i].w;
        }
    };
    dfs1(dfs1, 1, 0);

    auto dfs2 = [&](auto &&self, int u, int fa) -> void {
        for (int i = h[u]; i; i = e[i].ne) {
            int v = e[i].to;
            if (v == fa) {
                continue;
            }
            dp[v] = dp[u] + (e[i].w ? -1 : 1);
            self(self, v, u);
        }
    };
    dfs2(dfs2, 1, 0);

    int mx = 1E9;
    for (int i = 1; i <= n; i++) {
        mx = std::min(mx, dp[i]);
    }

    std::cout << mx << "\n";

    for (int i = 1; i <= n; i++) {
        if (dp[i] == mx) {
            std::cout << i << " ";
        }
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

# P2986 [USACO10MAR] Great Cow Gathering G

## 题目描述

Bessie 正在计划一年一度的奶牛大集会，来自全国各地的奶牛将来参加这一次集会。当然，她会选择最方便的地点来举办这次集会。

每个奶牛居住在 $N$ 个农场中的一个，这些农场由 $N-1$ 条道路连接，并且从任意一个农场都能够到达另外一个农场。道路 $i$ 连接农场 $A_i$ 和 $B_i$，长度为 $L_i$。集会可以在 $N$ 个农场中的任意一个举行。另外，每个牛棚中居住着 $C_i$ 只奶牛。

在选择集会的地点的时候，Bessie 希望最大化方便的程度（也就是最小化不方便程度）。比如选择第 $X$ 个农场作为集会地点，它的不方便程度是其它牛棚中每只奶牛去参加集会所走的路程之和（比如，农场 $i$ 到达农场 $X$ 的距离是 $20$，那么总路程就是 $C_i\times 20$）。帮助 Bessie 找出最方便的地点来举行大集会。

## 输入格式

第一行一个整数 $N$ 。

第二到 $N+1$ 行：第 $i+1$ 行有一个整数 $C_i$。

第 $N+2$ 行到 $2N$ 行：第 $i+N+1$ 行为 $3$ 个整数：$A_i,B_i$ 和 $L_i$。

## 输出格式

一行一个整数，表示最小的不方便值。

## 输入输出样例 #1

### 输入 #1

```
5 
1 
1 
0 
0 
2 
1 3 1 
2 3 2 
3 4 3 
4 5 3
```

### 输出 #1

```
15
```

## 说明/提示

$1\leq N\leq 10^5$，$1\leq A_i\leq B_i\leq N$，$0 \leq C_i,L_i \leq 10^3$。

# 题解

第一次**DFS**：

$siz[u]$表示以$u$为根的子树上奶牛的个数

$f[u]$表示以$u$为根的子树上，所有奶牛走到$u$的距离和

状态方程：$f[u]+=f[v]+siz[v]*w$

第二次**DFS**：

以$u$为根换到以$v$为根，考虑$f$值的变化：

- 在$v$子树上的奶牛不用走$w$这条边，所以$f$值要减去$siz[v]*w$
- 不在$v$子树上的奶牛必须走$w$这条边，$f$值要加上$(cnt-siz[v])*w$

得到状态转移方程：$f[v]=f[u]-siz[v]*w+(cnt-siz[v])*w$

更新$f$的最小值

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n;
    std::cin >> n;

    std::vector<int> c(n + 1);
    i64 sum = 0;
    for (int i = 1; i <= n; i++) {
        std::cin >> c[i];
        sum += c[i];
    }

    std::vector<std::vector<std::array<int, 2>>> adj(n + 1);
    for (int i = 1; i < n; i++) {
        int u, v, w;
        std::cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }

    std::vector<i64> siz(n + 1), f(n + 1);
    auto dfs1 = [&](auto &&self, int u, int fa) -> void {
        siz[u] = c[u];
        for (auto [v, w] : adj[u]) {
            if (v == fa) {
                continue;
            }
            self(self, v, u);
            siz[u] += siz[v];
            f[u] += f[v] + siz[v] * w;
        }
    };
    dfs1(dfs1, 1, 0);

    i64 ans = 1E18;
    auto dfs2 = [&](auto &&self, int u, int fa) -> void {
        for (auto [v, w] : adj[u]) {
            if (v == fa) {
                continue;
            }
            f[v] = f[u] - siz[v] * w + (sum - siz[v]) * w;//(sum-siz[v])除了v以外的其他牛都要走w这条边
            ans = std::min(ans, f[v]);
            self(self, v, u);
        }
    };
    dfs2(dfs2, 1, 0);

    std::cout << ans << "\n";
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

