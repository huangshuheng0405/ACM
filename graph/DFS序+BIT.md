# DFS序+树状数组

[题面](https://loj.ac/p/144)

- 1 a x，将结点a的权值增加x

- 2 a，求结点a的子树上所有结点的权值之和

  将一棵树平铺成一个数组，然后用**树状数组**维护单点修改和区间查询操作 
  
  用$dfs$遍历这棵树，进入$x$节点有一个$in$时间戳，递归退出时有一个$out$时间戳，$x$节点的两个时间戳之间遍历到的点，就是以$x$为根的子树的所有节点

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

template <typename T> struct Fenwick {
    int n;
    std::vector<T> a;

    Fenwick(int n_ = 0) { init(n_); }

    void init(int n_) {
        n = n_;
        a.assign(n, T{});
    }

    void add(int x, const T &v) {
        for (int i = x + 1; i <= n; i += i & -i) {
            a[i - 1] = a[i - 1] + v;
        }
    }

    T sum(int x) {
        T ans{};
        for (int i = x; i > 0; i -= i & -i) {
            ans = ans + a[i - 1];
        }
        return ans;
    }

    T rangeSum(int l, int r) { return sum(r) - sum(l); }

    int select(const T &k) {
        int x = 0;
        T cur{};
        for (int i = 1 << std::__lg(n); i; i /= 2) {
            if (x + i <= n && cur + a[x + i - 1] <= k) {
                x += i;
                cur = cur + a[x - 1];
            }
        }
        return x;
    }
};

void solve() {
    int n, m, s;
    cin >> n >> m >> s;
    vector<int> adj[n + 1], a(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    vector<int> l(n + 1), r(n + 1);
    int cnt = 0;
    Fenwick<i64> fen(n + 1);
    function<void(int, int)> dfs = [&](int u, int fa) {
        l[u] = ++cnt;
        fen.add(l[u], a[u]);
        for (auto v : adj[u]) {
            if (v == fa) {
                continue;
            }
            dfs(v, u);
        }
        r[u] = cnt;
    };
    dfs(s, 0);
    while (m--) {
        int op;
        cin >> op;
        if (op == 1) {
            int x, y;
            cin >> x >> y;
            fen.add(l[x], y);
        } else {
            int x;
            cin >> x;
            cout << fen.rangeSum(l[x], r[x] + 1) << "\n";
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

### 与上面不同的是将结点s的值改为x

```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main()
{
    int n, q;
    cin >> n >> q;
    vector<ll> a(n + 1), adj[n + 1], in(n + 1), out(n + 1), tr(n + 1);
    int cnt = 0;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
    }
    function<ll(ll)> lowbit = [&](ll x)
    {
        return x & -x;
    };
    function<void(ll, ll)> update = [&](ll x, ll val)
    {
        while (x <= n)
        {
            tr[x] += val;
            x += lowbit(x);
        }
    };
    function<ll(ll)> query = [&](ll x)
    {
        ll ans = 0;
        while (x)
        {
            ans += tr[x];
            x -= lowbit(x);
        }
        return ans;
    };
    function<void(int, int)> dfs = [&](int u, int fa)
    {
        in[u] = ++cnt;
        update(in[u], a[u]);//更新
        for (int v : adj[u])
        {
            if (v == fa)
            {
                continue;
            }
            dfs(v, u);
        }
        out[u] = cnt;
    };
    for (int i = 1; i < n; i++)
    {
        int a, b;
        cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }
    dfs(1, 0);
    while (q--)
    {
        int op, x, s;
        cin >> op;
        if (op == 1)//将结点s修改为x
        {
            cin >> s >> x;
            update(in[s], x - (query(in[s]) - query(in[s] - 1)));//因为是修改为x所以要减去之前的值
        }
        else//查询区间的和
        {
            cin >> s;
            cout << query(out[s]) - query(in[s] - 1) << endl;
        }
    }
}

```

