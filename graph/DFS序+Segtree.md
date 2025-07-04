# DFS序+线段树

[题面](https://loj.ac/p/145)

### 大意

- 1 a x,将结点$a$的子树上所有结点的权值增加$x$
- 2 a,求结点$a$的子树上所有结点的权值之和

# 题解

时间戳的性质，其中有一：$ ∀v∈Tree(u)，dfn(u)≤dfn(v)<dfn(u)+siz(u). $即$u$子树内所有节点$(v)$的$dfn$是连续的。

区间修改和区间查询用线段树维护

存图要用链式前向星，邻接表会`MLE`

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

#define lc x << 1
#define rc x << 1 | 1

constexpr int N = 1e6 + 5;
struct node {
    int l, r, tag;
    i64 s;
} a[N << 2];
int w[N], num[N];

void pushup(int x) { a[x].s = a[lc].s + a[rc].s; }

void pushdown(int x) {
    if (a[x].tag) {
        a[lc].tag += a[x].tag;
        a[rc].tag += a[x].tag;
        a[lc].s += 1ll * (a[lc].r - a[lc].l + 1) * a[x].tag;
        a[rc].s += 1ll * (a[rc].r - a[rc].l + 1) * a[x].tag;
        a[x].tag = 0;
    }
}

void build(int x, int l, int r) {
    a[x].l = l;
    a[x].r = r;
    if (l == r) {
        a[x].s = w[num[l]];
        return;
    }
    int mid = (l + r) >> 1;
    build(lc, l, mid);
    build(rc, mid + 1, r);
    pushup(x);
}

void modify(int x, int l, int r, int val) {
    if (l <= a[x].l && a[x].r <= r) {
        a[x].s += 1ll * (a[x].r - a[x].l + 1) * val;
        a[x].tag += val;
        return;
    }
    pushdown(x);
    int mid = (a[x].l + a[x].r) >> 1;
    if (l <= mid) {
        modify(lc, l, r, val);
    }
    if (r > mid) {
        modify(rc, l, r, val);
    }
    pushup(x);
}

i64 query(int x, int l, int r) {
    if (l <= a[x].l && a[x].r <= r) {
        return a[x].s;
    }
    int mid = (a[x].l + a[x].r) >> 1;
    pushdown(x);
    i64 res = 0;
    if (l <= mid) {
        res += query(lc, l, r);
    }
    if (r > mid) {
        res += query(rc, l, r);
    }
    return res;
}

void solve() {
    int n, m, s;
    cin >> n >> m >> s;
    for (int i = 1; i <= n; i++) {
        cin >> w[i];
    }
    int cur = 0;
    vector<int> ne(2 * n + 1), to(2 * n + 1), h(n + 1);
    auto add = [&](int u, int v) {
        to[++cur] = v;
        ne[cur] = h[u];
        h[u] = cur;
    };
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        add(u, v);
        add(v, u);
    }
    vector<int> in(n + 1), out(n + 1);
    int cnt = 0;
    function<void(int, int)> dfs = [&](int u, int fa) {
        in[u] = ++cnt;
        num[cnt] = u; // 记录每个编号对应的结点 便于初始化树状数组
        for (int i = h[u]; i; i = ne[i]) {
            int v = to[i];
            if (v == fa) {
                continue;
            }
            dfs(v, u);
        }
        out[u] = cnt;
    };
    dfs(s, 0);
    build(1, 1, n);
    while (m--) {
        int op;
        cin >> op;
        if (op == 1) {
            int x, y;
            cin >> x >> y;
            modify(1, in[x], out[x], y);
        } else {
            int x;
            cin >> x;
            cout << query(1, in[x], out[x]) << "\n";
        }
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

