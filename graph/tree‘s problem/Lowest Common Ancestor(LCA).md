# 最近公共祖先

[LCA](https://oi-wiki.org/graph/lca/)

# 【模板】最近公共祖先（LCA）

## 题目描述

如题，给定一棵有根多叉树，请求出指定两个点直接最近的公共祖先。

## 输入格式

第一行包含三个正整数 $N,M,S$，分别表示树的结点个数、询问的个数和树根结点的序号。

接下来 $N-1$ 行每行包含两个正整数 $x, y$，表示 $x$ 结点和 $y$ 结点之间有一条直接连接的边（数据保证可以构成树）。

接下来 $M$ 行每行包含两个正整数 $a, b$，表示询问 $a$ 结点和 $b$ 结点的最近公共祖先。

## 输出格式

输出包含 $M$ 行，每行包含一个正整数，依次为每一个询问的结果。

## 样例 #1

### 样例输入 #1

```
5 5 4
3 1
2 4
5 1
1 4
2 4
3 2
3 5
1 2
4 5
```

### 样例输出 #1

```
4
4
1
4
4
```

## 提示

对于 $30\%$ 的数据，$N\leq 10$，$M\leq 10$。

对于 $70\%$ 的数据，$N\leq 10000$，$M\leq 10000$。

对于 $100\%$ 的数据，$1 \leq N,M\leq 500000$，$1 \leq x, y,a ,b \leq N$，**不保证** $a \neq b$。


样例说明：

该树结构如下：

 ![](https://cdn.luogu.com.cn/upload/pic/2282.png) 

第一次询问：$2, 4$ 的最近公共祖先，故为 $4$。

第二次询问：$3, 2$ 的最近公共祖先，故为 $4$。

第三次询问：$3, 5$ 的最近公共祖先，故为 $1$。

第四次询问：$1, 2$ 的最近公共祖先，故为 $4$。

第五次询问：$4, 5$ 的最近公共祖先，故为 $4$。

故输出依次为 $4, 4, 1, 4, 4$。

2021/10/4 数据更新 @fstqwq：应要求加了两组数据卡掉了暴力跳。

# Tarjan

tarjan是一种**离线算法**（需要事先知道全部答案），利用并查集维护祖先节点

`adj`：存边

`query`：存查询，$query[3]= \{4,1\}$，表示带查询的结点是3和4，查询编号为1

`fa`：存父节点

`vis`：打标记

`ans`：存查询结果

#### 过程

1. 从根节点开始深搜遍历，入u时打标记

2. 枚举u的子节点v，遍历完v的子树，回u时，把v指向u

3. 遍历完u的子节点，离开u时，枚举以u为起点的查询，若终点v被搜过，则查找v的祖先节点，即u，v的LCA，答案记录到ans[i]

4. 递归遍历完整棵树，得到全部查询的答案

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, m, s;
    cin >> n >> m >> s;
    vector<int> adj[n + 1], ans(m + 1), fa(n + 1);
    vector<bool> vis(n + 1);
    iota(fa.begin(), fa.end(), 0); // 初始化父节点
    vector<tuple<int, int>> query[n + 1];
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for (int i = 1; i <= m; i++) {
        int x, y; // 双向建边 有可能正着建得时候那个点还没被访问过
        cin >> x >> y;
        query[x].push_back({y, i});
        query[y].push_back({x, i});
    }
    function<int(int)> find = [&](int x) {
        return x == fa[x] ? x : fa[x] = find(fa[x]);
    };
    function<void(int)> dfs = [&](int u) {
        vis[u] = true;
        for (auto v : adj[u]) {
            if (!vis[v]) {
                dfs(v);
                fa[v] = u; // 回到u时 v的父节点指向u
            }
        }
        for (auto [v, i] : query[u]) { // 离开u时 枚举查询
            if (vis[v]) {
                ans[i] = find(v);
            }
        }
    };
    dfs(s);
    for (int i = 1; i <= m; i++) {
        cout << ans[i] << "\n";
    }
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

# 倍增算法

$dep[u]$存u点的深度

$fa[u][i]$存从u点向上跳$2^i$层的祖先结点，$i=0,1,2,3\dots$ 

#### 1.dfs遍历一遍创建ST表

每次向上跳$2^i$层，分两次跳,每次跳$2^{i-1}$层，第一次跳到$fa[u][i-1]$,再从$fa[u][i-1]$跳到$fa [fa[u][i-1]][i-1]$

#### 2.利用ST表求LCA

1. 将u，v跳到同一层，设u，v两点的深度之差为y，将y进行二进制拆分，一定能跳到同一层（y一定能被二进制表示）

2. 将u，v一起跳到LCA的下一层

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

struct Tree {
    int n;
    vector<vector<int>> ver, val;
    vector<int> lg, dep;
    Tree(int n) {
        this->n = n;
        ver.resize(n + 1);
        val.resize(n + 1, vector<int>(30));
        lg.resize(n + 1);
        dep.resize(n + 1);
        for (int i = 1; i <= n; i++) { // 预处理 log
            lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
        }
    }
    void add(int x, int y) { // 建立双向边
        ver[x].push_back(y);
        ver[y].push_back(x);
    }
    void dfs(int x, int fa) {
        val[x][0] = fa; // 储存 x 的父节点
        dep[x] = dep[fa] + 1;
        for (int i = 1; i <= lg[dep[x]]; i++) {
            val[x][i] = val[val[x][i - 1]][i - 1];
        }
        for (auto y : ver[x]) {
            if (y == fa)
                continue;
            dfs(y, x);
        }
    }
    int lca(int x, int y) {
        if (dep[x] < dep[y])
            swap(x, y);
        while (dep[x] > dep[y]) {
            x = val[x][lg[dep[x] - dep[y]] - 1];
        }
        if (x == y)
            return x;
        for (int k = lg[dep[x]] - 1; k >= 0; k--) {
            if (val[x][k] == val[y][k])
                continue;
            x = val[x][k];
            y = val[y][k];
        }
        return val[x][0];
    }
    int clac(int x, int y) { // 倍增查询两点间距离
        return dep[x] + dep[y] - 2 * dep[lca(x, y)];
    }
    void work(int root = 1) { // 在此初始化
        dfs(root, 0);
    }
};

void solve() {
    int n, m, s;
    cin >> n >> m >> s;
    Tree tree(n + 1);

    // 建图
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        tree.add(u, v);
    }
    tree.dfs(s, 0);
    // 处理查询
    while (m--) {
        int a, b;
        cin >> a >> b;
        cout << tree.lca(a, b) << "\n";
    }
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

# 树链剖分

**重儿子**：父节点所有儿子中子树节点数目最多的结点

轻儿子：父节点除重儿子以外的儿子

重边：父节点和重儿子连成的边

轻边：父节点和轻儿子连成的边

**重链**：有多条重边连接而成的路径

$fa$：存父节点

$dep$：存深度

$son$：存重儿子

$siz$：存子树的结点数

$top$：存所在重链的顶点

### 过程

第一遍dfs，算出$fa,dep,son$数组

第二遍dfs，算出$top$数组

LCA为两个游标跳转到同一条重链上时深度较小的那个游标所指向的点

树链剖分的预处理时间复杂度为$O(n)$，单词查询的时间复杂度为$O(logn)$，并且常数非常小

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, m, s;
    cin >> n >> m >> s;
    vector<int> siz(n + 1), dep(n + 1), top(n + 1), fa(n + 1), son(n + 1),
        adj[n + 1];
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    function<void(int, int)> dfs1 = [&](int u, int f) -> void {
        fa[u] = f;
        dep[u] = dep[f] + 1;
        siz[u] = 1;
        for (auto v : adj[u]) {
            if (v == f) {
                continue;
            }
            dfs1(v, u); // 调用具名递归函数
            siz[u] += siz[v];
            if (siz[son[u]] < siz[v]) { // 更新重儿子
                son[u] = v;
            }
        }
    };

    function<void(int, int)> dfs2 = [&](int u, int t) -> void { // t为链头
        top[u] = t;                                             // 记录链头
        if (!son[u]) { // 叶节点无重儿子 返回
            return;
        }
        dfs2(son[u], t); // 搜重儿子
        for (auto v : adj[u]) {
            if (v == fa[u] || v == son[u]) {
                continue;
            }
            dfs2(v, v); // 搜轻儿子
        }
    };

    dfs1(s, 0); // 第一次 DFS，从 s 出发
    dfs2(s, s); // 第二次 DFS，设置链头

    auto lca = [&](int u, int v) -> int {
        while (top[u] != top[v]) {
            if (dep[top[u]] < dep[top[v]]) {
                swap(u, v);
            }
            u = fa[top[u]]; // u跳到链头的父节点
        }
        return dep[u] < dep[v] ? u : v;
    };

    while (m--) {
        int x, y;
        cin >> x >> y;
        cout << lca(x, y) << "\n";
    }
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

板子

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

struct HLD {
    int n, idx;
    vector<vector<int>> adj;
    vector<int> siz, dep;
    vector<int> top, son, parent;

    HLD(int n) {
        this->n = n;
        adj.resize(n + 1);
        siz.resize(n + 1);
        dep.resize(n + 1);
        top.resize(n + 1);
        son.resize(n + 1);
        parent.resize(n + 1);
    }
    void add(int x, int y) { // 建立双向边
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    void dfs1(int x) {
        siz[x] = 1;
        dep[x] = dep[parent[x]] + 1;
        for (auto y : adj[x]) {
            if (y == parent[x])
                continue;
            parent[y] = x;
            dfs1(y);
            siz[x] += siz[y];
            if (siz[y] > siz[son[x]]) {
                son[x] = y;
            }
        }
    }
    void dfs2(int x, int up) {
        top[x] = up;
        if (son[x])
            dfs2(son[x], up);
        for (auto y : adj[x]) {
            if (y == parent[x] || y == son[x])
                continue;
            dfs2(y, y);
        }
    }
    int lca(int x, int y) {
        while (top[x] != top[y]) {
            if (dep[top[x]] > dep[top[y]]) {
                x = parent[top[x]];
            } else {
                y = parent[top[y]];
            }
        }
        return dep[x] < dep[y] ? x : y;
    }
    int clac(int x, int y) { // 查询两点间距离
        return dep[x] + dep[y] - 2 * dep[lca(x, y)];
    }
    void work(int root = 1) { // 在此初始化
        dfs1(root);
        dfs2(root, root);
    }
};

void solve() {
    int n, m, s;
    cin >> n >> m >> s;
    HLD hld(n);
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        hld.add(u, v);
    }
    hld.work(s);
    while (m--) {
        int x, y;
        cin >> x >> y;
        cout << hld.lca(x, y) << "\n";
    }
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



