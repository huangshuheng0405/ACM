# 欧拉回路

# 定义

- **欧拉回路**：通过图中每条边恰好一次的回路
- **欧拉通路**：通过图中恰好一次的通路
- **欧拉图**：具有欧拉回路的图
- **具有欧拉**：通路但不具有欧拉回路的图

# 性质

欧拉图中所有顶点的度数都是偶数。

若 $G$ 是欧拉图，则它为若干个环的并，且每条边被包含在奇数个环内。

# 判定

- **有向图欧拉路径** ：图中**恰好**存在 $1$个点出度比入度多 $1$（这个点即为 **起点** $S$），$1$个点入度比出度多$1$（这个点即为 **终点** $T$），其余节点出度=入度。 
- **有向图欧拉回路**：所有点的入度=出度（起点和终点可以是任意点）
- **无向图欧拉路径**：图中恰好存在2个点的度数是奇数，其余节点的度数是偶数，这两个度数为奇数的点即为欧拉回路的起点$S$和终点$T$
- **无向图欧拉回路**：所有点的度数都是偶数（起点和终点可以是任意点）

存在欧拉回路一定存在欧拉路径，因为 图中经过所有边**恰好一次**的路径叫**欧拉路径**（也就是一笔画）。如果此路径的**起点**和**终点**相同，则称其为一条**欧拉回路**。 

# 【模板】欧拉路径

## 题目描述

求有向图字典序最小的欧拉路径。

## 输入格式

第一行两个整数 $n,m$ 表示有向图的点数和边数。

接下来 $m$ 行每行两个整数 $u,v$ 表示存在一条 $u\to v$ 的有向边。

## 输出格式

如果不存在欧拉路径，输出一行 `No`。

否则输出一行 $m+1$ 个数字，表示字典序最小的欧拉路径。

## 样例 #1

### 样例输入 #1

```
4 6
1 3
2 1
4 2
3 3
1 2
3 4
```

### 样例输出 #1

```
1 2 1 3 3 4 2
```

## 样例 #2

### 样例输入 #2

```
5 5
1 2
3 5
4 3
3 4
2 3
```

### 样例输出 #2

```
1 2 3 4 3 5
```

## 样例 #3

### 样例输入 #3

```
4 3
1 2
1 3
1 4
```

### 样例输出 #3

```
No
```

## 提示

对于 $50\%$ 的数据，$n,m\leq 10^3$。

对于 $100\%$ 的数据，$1\leq u,v\leq n\leq 10^5$，$m\leq 2\times 10^5$。

保证将有向边视为无向边后图连通。

[数据生成器](https://www.luogu.com.cn/paste/9oswk47n)

# 题解

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

vector<int> adj[100005];
int du[100005][2]; // 记录出度和入度 0入度 1出度
int del[100005];   // 标记已访问过的点

void solve() {
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= m; i++) {
        int u, v;
        cin >> u >> v;

        adj[u].push_back(v);
        du[u][1]++;
        du[v][0]++;
    }

    for (int i = 1; i <= n; i++) {
        sort(adj[i].begin(), adj[i].end());  //输出字典序最小的欧拉路径
    }

    int S = 1, cnt[2] = {0, 0}; // S:dfs起点
    bool ok = false;
    for (int i = 1; i <= n; i++) {
        if (du[i][1] != du[i][0]) {
            ok = true;                      // 不存在欧拉回路了
            if (du[i][1] - du[i][0] == 1) { // 出度比入度多1
                cnt[1]++;
                S = i;
            } else if (du[i][0] - du[i][1] == 1) { // 入度比出度多1
                cnt[0]++;
            } else { // 入度与出度不同且也不差1 不满足条件直接输出No
                cout << "No\n";
                return;
            }
        }
    }

    if (ok && !(cnt[1] == cnt[0] && cnt[0] == 1)) { // 不满足欧拉回路和欧拉路径的条件
        cout << "No\n";
        return;
    }

    stack<int> stk;
    auto dfs = [&](auto &&self, int u) -> void {
        for (int i = del[u]; i < adj[u].size(); i = del[u]) {
            del[u] = i + 1;
            self(self, adj[u][i]);
        }
        stk.push(u);
    };

    dfs(dfs, S); // 从S开始DFS

    while (!stk.empty()) {
        cout << stk.top() << " ";
        stk.pop();
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

# 无序字母对

## 题目描述

给定 $n$ 个各不相同的无序字母对（区分大小写，无序即字母对中的两个字母可以位置颠倒）。请构造一个有 $(n+1)$ 个字母的字符串使得每个字母对都在这个字符串中出现。

## 输入格式

第一行输入一个正整数 $n$。

第二行到第 $(n+1)$ 行每行两个字母，表示这两个字母需要相邻。

## 输出格式

输出满足要求的字符串。

如果没有满足要求的字符串，请输出 `No Solution`。

如果有多种方案，请输出字典序最小的方案（即满足前面的字母的 ASCII 编码尽可能小）。

## 样例 #1

### 样例输入 #1

```
4
aZ
tZ
Xt
aX
```

### 样例输出 #1

```
XaZtX
```

## 提示

不同的无序字母对个数有限，$n$ 的规模可以通过计算得到。

# 题解

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

struct DSU {
    vector<int> f, siz;
    DSU() {}
    DSU(int n) { init(n); }
    void init(int n) {
        f.resize(n);
        iota(f.begin(), f.end(), 0);
        siz.assign(n, 1);
    }
    int find(int x) {
        while (x != f[x]) {
            x = f[x] = f[f[x]];
        }
        return x;
    }
    bool same(int x, int y) { return find(x) == find(y); }
    bool merge(int x, int y) {
        x = find(x);
        y = find(y);
        if (x == y) {
            return false;
        }
        siz[x] += siz[y];
        f[y] = x;
        return true;
    }
    int size(int x) { return siz[find(x)]; }
};

int adj[200][200], du[200];
char ans[1330];

void solve() {
    int n;
    cin >> n;

    DSU dsu(130);

    for (int i = 1; i <= n; i++) {
        string s;
        cin >> s;

        adj[s[0]][s[1]] = adj[s[1]][s[0]] = 1;
        du[s[0]]++;
        du[s[1]]++;
        dsu.merge(s[0], s[1]);
    }

    int cnt = 0;
    for (int i = 64; i <= 125; i++) {
        if (dsu.f[i] == i && du[i]) { // 祖宗节点
            cnt++;
        }
    }

    if (cnt != 1) { // 判断是否为联通图
        cout << "No Solution\n";
        return;
    }

    int S = 0;
    cnt = 0;

    for (int i = 64; i <= 125; i++) {
        if (du[i] & 1) { // 度是奇数为起点或终点
            cnt++;
            if (S == 0) {
                S = i; // 记录起点
            }
        }
    }

    if (cnt && cnt != 2) { // 如果有奇数个点并且不是两个说明不存在回路
        cout << "No Solution\n";
        return;
    }

    if (S == 0) { // 欧拉回路 所有点的度都是偶数
        for (int i = 64; i <= 125; i++) {
            if (du[i]) { // 找起点
                S = i;
                break;
            }
        }
    }

    auto dfs = [&](auto &&self, int u) -> void {
        for (int i = 64; i <= 125; i++) {
            if (adj[u][i]) {
                adj[u][i] = adj[i][u] = 0;
                self(self, i);
            }
        }
        ans[n--] = u;
    };

    dfs(dfs, S);

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

