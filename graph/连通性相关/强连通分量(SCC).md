# 强连通分量

### 定义

有向图$G$强连通是指，$G$中任意两个结点连通

### Tarjan算法

时间复杂度：$O(n+m)$

#### 过程

时间戳$dfn_x$：节点$u$第一次被访问的顺序

追溯值$low_x$：从节点出发$x$出发。所能访问到的最早时间戳

按照深度优先搜索算法搜索的次序对图中所有的结点进行搜索，维护每个结点的 `dfn` 与 `low` 变量，且让搜索到的结点入栈。每当找到一个强连通元素，就按照该元素包含结点数目让栈中元素出栈。在搜索过程中，对于结点$u$和与其相邻的结点$v$（$v$不是$u$的父节点）考虑3中情况：

1. $y$未被访问：继续对$v$进行深度搜索。在回溯过程中，用$\large low_x$更新$\large low_x$。因为$x$是$y$的父节点，$y$能访问到的点，$x$也一定能访问到
2. $y$为访问过且在栈中：说明$y$是祖先节点或左子树节点，用$\large dfn_y$更新$\large low_x$。
3. $y$被访问过且不在栈中：说明$y$所在的连通分量已经被处理，不用进行操作

离开$x$时，记录SCC，只有遍历完一个SCC，才可以出栈。更新$low$值的意义：**避免SCC的节点提前出栈**

对于一个SCC,该连通图有且仅有一个$u$是的$dfn_u=low_u$.该结点一定是在深度遍历的过程中,该连通分量中第一个被访问过的结点,因为它的$dfn$和$low$值最小,不会被该连通分量中的其他结点所影响

因此,在回溯的过程中,判定$dfn_u=low_u$是否成立,如果成立,则栈中$u$及其上方的结点构成一个SCC

### 应用

我们可以将一张图的每个强连通分量都缩成一个点。

然后这张图会变成一个 DAG，可以进行拓扑排序以及更多其他操作。

举个简单的例子，求一条路径，可以经过重复结点，要求经过的不同结点数量最多。

### 板子

```c++
struct SCC {
    int n;
    std::vector<std::vector<int>> adj;
    std::vector<int> stk;
    std::vector<int> dfn, low, bel, siz;//belong
    int cur, cnt;
    SCC() {}
    SCC(int n) { init(n); }
    void init(int n) {
        this->n = n;
        adj.assign(n, {});
        dfn.assign(n, -1);
        low.resize(n);
        bel.assign(n, -1); // SCC编号
        siz.resize(n);     // 每个SCC的点的数量
        stk.clear();
        cur = cnt = 0;     //cnt表示有几个SCC
    }
    void addEdge(int u, int v) { adj[u].push_back(v); }
    void dfs(int x) {
        dfn[x] = low[x] = ++cur;
        stk.push_back(x);
        for (auto y : adj[x]) {
            if (dfn[y] == -1) {//若y没遍历过
                dfs(y);
                low[x] = std::min(low[x], low[y]);//用子节点更新low值
            } else if (bel[y] == -1) {//y已访问
                low[x] = std::min(low[x], dfn[y]);
            }
        }
        if (dfn[x] == low[x]) {//若x时SCC的根
            int y;
            cnt++;
            do {
                y = stk.back();
                stk.pop_back();
                bel[y] = cnt;//y点是第几个强连通分量
                siz[cnt]++;//第cnt个强连通分量的点数+1
            } while (y != x);
        }
    }
    std::vector<int> work() {
        for (int i = 0; i < n; i++) {
            if (dfn[i] == -1) {
                dfs(i);
            }
        }
        return bel;
    }
};
```



# [USACO06JAN] The Cow Prom S

## 题目描述

有一个 $n$ 个点，$m$ 条边的有向图，请求出这个图点数大于 $1$ 的强连通分量个数。

## 输入格式

第一行为两个整数 $n$ 和 $m$。

第二行至 $m+1$ 行，每一行有两个整数 $a$ 和 $b$，表示有一条从 $a$ 到 $b$ 的有向边。

## 输出格式

仅一行，表示点数大于 $1$ 的强连通分量个数。

## 样例 #1

### 样例输入 #1

```
5 4
2 4
3 5
1 2
4 1
```

### 样例输出 #1

```
1
```

## 提示

#### 数据规模与约定

对于全部的测试点，保证 $2\le n \le 10^4$，$2\le m\le 5\times 10^4$，$1 \leq a, b \leq n$。

# 题解

`scc`存每个点是第几个强连通分量

`siz`存的是第$cnt$个强连通分量所含点的个数

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

struct SCC {
    int n;
    vector<vector<int>> adj;
    vector<int> stk;
    vector<int> dfn, low, bel, siz;
    int cur, cnt;
    SCC() {}
    SCC(int n) { init(n); }
    void init(int n) {
        this->n = n;
        adj.assign(n, {});
        dfn.assign(n, -1);
        low.resize(n);
        bel.assign(n, -1); // SCC编号
        siz.resize(n);     // 每个SCC的点的数量
        stk.clear();
        cur = cnt = 0;
    }
    void addEdge(int u, int v) { adj[u].push_back(v); }
    void dfs(int x) {
        dfn[x] = low[x] = ++cur;
        stk.push_back(x);
        for (auto y : adj[x]) {
            if (dfn[y] == -1) {
                dfs(y);
                low[x] = min(low[x], low[y]);
            } else if (bel[y] == -1) {
                low[x] = min(low[x], dfn[y]);
            }
        }
        if (dfn[x] == low[x]) {
            int y;
            cnt++;
            do {
                y = stk.back();
                bel[y] = cnt;
                stk.pop_back();
                siz[cnt]++;
            } while (y != x);
        }
    }
    vector<int> work() {
        for (int i = 0; i < n; i++) {
            if (dfn[i] == -1) {
                dfs(i);
            }
        }
        return bel;
    }
};

void solve() {
    int n, m;
    cin >> n >> m;

    SCC scc(n + 1);
    for (int i = 1; i <= m; i++) {
        int u, v;
        cin >> u >> v;
        scc.addEdge(u, v);
    }
    for (int i = 1; i <= n; i++) {
        if (scc.dfn[i] == -1) {
            scc.dfs(i);
        }
    }
    int ans = 0;
    for (int i = 1; i <= scc.cnt; i++) {
        if (scc.siz[i] > 1) {
            ans++;
        }
    }
    cout << ans << "\n";
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

# 间谍网络

## 题目描述

由于外国间谍的大量渗入，国家安全正处于高度的危机之中。如果 A 间谍手中掌握着关于 B 间谍的犯罪证据，则称 A 可以揭发 B。有些间谍收受贿赂，只要给他们一定数量的美元，他们就愿意交出手中掌握的全部情报。所以，如果我们能够收买一些间谍的话，我们就可能控制间谍网中的每一分子。因为一旦我们逮捕了一个间谍，他手中掌握的情报都将归我们所有，这样就有可能逮捕新的间谍，掌握新的情报。

我们的反间谍机关提供了一份资料，包括所有已知的受贿的间谍，以及他们愿意收受的具体数额。同时我们还知道哪些间谍手中具体掌握了哪些间谍的资料。假设总共有 $n$ 个间谍（$n$ 不超过 $3000$），每个间谍分别用 $1$ 到 $3000$ 的整数来标识。

请根据这份资料，判断我们是否有可能控制全部的间谍，如果可以，求出我们所需要支付的最少资金。否则，输出不能被控制的一个间谍。

## 输入格式

第一行只有一个整数 $n$。

第二行是整数 $p$。表示愿意被收买的人数，$1\le p\le n$。

接下来的 $p$ 行，每行有两个整数，第一个数是一个愿意被收买的间谍的编号，第二个数表示他将会被收买的数额。这个数额不超过 $20000$。

紧跟着一行只有一个整数 $r$，$1\le r\le8000$。然后 $r$ 行，每行两个正整数，表示数对 $(A, B)$，$A$ 间谍掌握 $B$ 间谍的证据。

## 输出格式

如果可以控制所有间谍，第一行输出 `YES`，并在第二行输出所需要支付的贿金最小值。否则输出 `NO`，并在第二行输出不能控制的间谍中，编号最小的间谍编号。

## 样例 #1

### 样例输入 #1

```
3
2
1 10
2 100
2
1 3
2 3
```

### 样例输出 #1

```
YES
110
```

## 样例 #2

### 样例输入 #2

```
4
2
1 100
4 200
2
1 2
3 4
```

### 样例输出 #2

```
NO
3
```

# 题解

买入度为0的点，但是如果x指向y，但是x是我们不能买的 或者 x不能被其他点到达 那么y一定要买

无解情况：某个SCC不能买

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

vector<int> w(3005, 1E9);//初始化每个间谍都不能被收买
struct SCC {
    int n;
    vector<vector<int>> adj;
    vector<int> stk;
    vector<int> dfn, low, bel, siz, mi, minId;
    int cur, cnt;
    SCC() {}
    SCC(int n) { init(n); }
    void init(int n) {
        this->n = n;
        adj.assign(n, {});
        dfn.assign(n, -1);
        low.resize(n);
        bel.assign(n, -1); // SCC编号
        siz.resize(n);     // 每个SCC的点的数量
        stk.clear();
        mi.resize(n);
        minId.resize(n);
        cur = cnt = 0;
    }
    void addEdge(int u, int v) { adj[u].push_back(v); }
    void dfs(int x) {
        dfn[x] = low[x] = ++cur;
        stk.push_back(x);
        for (auto y : adj[x]) {
            if (dfn[y] == -1) { // 若y没遍历过
                dfs(y);
                low[x] = min(low[x], low[y]); // 用子节点更新low值
            } else if (bel[y] == -1) {        // y已访问
                low[x] = min(low[x], dfn[y]);
            }
        }
        if (dfn[x] == low[x]) { // 若x时SCC的根
            int y;
            int minw = 1E9, lk = 1E9;
            cnt++;
            do {
                y = stk.back();
                stk.pop_back();
                bel[y] = cnt; // y点是第几个强连通分量
                siz[cnt]++;   // 第cnt个强连通分量的点数+1
                minw = min(minw, w[y]);//记录这个SCC的最小花费
                lk = min(lk, y);//记录这个SCC最小编号
            } while (y != x);
            minId[cnt] = lk;
            mi[cnt] = minw;
        }
    }
    vector<int> work() {
        for (int i = 0; i < n; i++) {
            if (dfn[i] == -1) {
                dfs(i);
            }
        }
        return bel;
    }
};

bool vis[3005][3005];

void solve() {
    int n, p;
    cin >> n >> p;
    for (int i = 1; i <= p; i++) {
        int u, v;
        cin >> u >> v;
        w[u] = v;
    }
    int r;
    cin >> r;
    SCC scc(n + 1);
    for (int i = 1; i <= r; i++) {
        int u, v;
        cin >> u >> v;
        scc.addEdge(u, v);
    }
    for (int i = 1; i <= n; i++) {
        if (scc.dfn[i] == -1) {
            scc.dfs(i);
        }
    }
    auto bel = scc.bel;
    vector<bool> in(n + 1);
    for (int u = 1; u <= n; u++) {
        for (auto v : scc.adj[u]) {
            if (bel[u] == bel[v] || vis[bel[u]][bel[v]]) {//同一个SCC 或 这个两个SCC已经判断过
                continue;
            }
            vis[bel[u]][bel[v]] = true;
            if (in[bel[u]] || scc.mi[bel[u]] != 1E9) {//如果u有入度那么v也有入度 如果u所在SCC能被贿赂
                in[bel[v]] = true;
            }
        }
    }
    int ans = 1E9, all = 0;
    bool ok = false;
    for (int i = 1; i <= scc.cnt; i++) {
        if (in[i]) {//有入度就不用判断
            continue;
        }
        if (scc.mi[i] == 1E9) {
            ok = true;
            ans = min(ans, scc.minId[i]);//记录最小编号
        }
        all += scc.mi[i];
    }
    if (ok) {
        cout << "NO\n" << ans << "\n";
    } else {
        cout << "YES\n" << all << "\n";
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

# B3609 [图论与代数结构 701] 强连通分量

## 题目描述

给定一张 $n$ 个点 $m$ 条边的有向图，求出其所有的强连通分量。

**注意，本题可能存在重边和自环。**

## 输入格式

第一行两个正整数 $n$ ， $m$ ，表示图的点数和边数。

接下来 $m$ 行，每行两个正整数 $u$ 和 $v$ 表示一条边。

## 输出格式

第一行一个整数表示这张图的强连通分量数目。

接下来每行输出一个强连通分量。第一行输出 1 号点所在强连通分量，第二行输出 2 号点所在强连通分量，若已被输出，则改为输出 3 号点所在强连通分量，以此类推。每个强连通分量按节点编号大小输出。

## 输入输出样例 #1

### 输入 #1

```
6 8
1 2
1 5
2 6
5 6
6 1
5 3
6 4
3 4
```

### 输出 #1

```
3
1 2 5 6
3
4
```

## 说明/提示

对于所有数据，$1 \le n \le 10000$，$1 \le m \le 100000$。

# 题解

```c++
#include <bits/stdc++.h>

using i64 = long long;

struct SCC {
    int n;
    std::vector<std::vector<int>> adj, ans;
    std::vector<int> stk;
    std::vector<int> dfn, low, bel, siz; // belong
    int cur, cnt;
    SCC() {}
    SCC(int n) { init(n); }
    void init(int n) {
        this->n = n;
        adj.assign(n, {});
        ans.assign(n, {});
        dfn.assign(n, -1);
        low.resize(n);
        bel.assign(n, -1); // SCC编号
        siz.resize(n);     // 每个SCC的点的数量
        stk.clear();
        cur = cnt = 0;
    }
    void addEdge(int u, int v) { adj[u].push_back(v); }
    void dfs(int x) {
        dfn[x] = low[x] = ++cur;
        stk.push_back(x);
        for (auto y : adj[x]) {
            if (dfn[y] == -1) { // 若y没遍历过
                dfs(y);
                low[x] = std::min(low[x], low[y]); // 用子节点更新low值
            } else if (bel[y] == -1) {             // y已访问
                low[x] = std::min(low[x], dfn[y]);
            }
        }
        if (dfn[x] == low[x]) { // 若x时SCC的根
            int y;
            cnt++;
            do {
                y = stk.back();
                stk.pop_back();
                bel[y] = cnt; // y点是第几个强连通分量
                siz[cnt]++;   // 第cnt个强连通分量的点数+1
                ans[cnt].push_back(y);
            } while (y != x);
        }
    }
    std::vector<int> work() {
        for (int i = 0; i < n; i++) {
            if (dfn[i] == -1) {
                dfs(i);
            }
        }
        return bel;
    }
};

void solve() {
    int n, m;
    std::cin >> n >> m;

    SCC scc(n + 1);
    for (int i = 0; i < m; i++) {
        int u, v;
        std::cin >> u >> v;
        scc.adj[u].push_back(v);
    }

    for (int i = 1; i <= n; i++) {
        if (scc.dfn[i] == -1) {
            scc.dfs(i);
        }
    }

    std::cout << scc.cnt << '\n';

    std::vector<bool> vis(n + 1);
    for (int i = 1; i <= n; i++) {
        int x = scc.bel[i];
        if (vis[x]) {
            continue;
        }
        vis[x] = true;
        auto ans = scc.ans[x];
        std::sort(ans.begin(), ans.end());
        for (auto i : ans) {
            std::cout << i << " ";
        }
        std::cout << "\n";
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);
    int _ = 1;

    // std::cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

