# DFS序+树状数组

- ### 1 a x，表示将结点a的权值增加x

- ### 2 a，表示结点a的子树上所有结点的权值之和

  将一棵树平铺成一个数组，然后用**树状数组**维护单点修改和区间查询操作 

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<cmath>
#include<set>
#include<map>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e6 + 5, mod = 1e9 + 7;
ll t, n, q, root, a[N], in[N], on[N], cnt, tr[N], op, k, y;
vector<int> e[N];

ll lowbit(ll x) {
    return x & -x;
}

void updata(ll x, ll val) {//向后更新
    while (x <= n) {
        tr[x] += val;
        x += lowbit(x);
    }
}

ll query(ll x) {// 向前查前缀和
    ll ans = 0;
    while (x) {
        ans += tr[x];
        x -= lowbit(x);
    }
    return ans;
}

void dfs(int u, int fa) {
    in[u] = ++cnt;// 记录以u为子树的左边界
    updata(in[u], a[u]);
    for (int v: e[u]) {
        if (v == fa)//双向边不能回头
            continue;
        dfs(v, u);
    }
    on[u] = cnt; //记录右边界
}

void solve() {
    cin >> n >> q >> root;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    int u, v;
    for (int i = 1; i <= n - 1; i++) {
        cin >> u >> v;
        e[u].push_back(v);
        e[v].push_back(u);
    }
    dfs(root, 0);
    while (q--) {
        cin >> op;
        if (op == 1) {
            cin >> y >> k;
            updata(in[y], k);//从左端点更新 
        } else {
            cin >> y;
            cout << query(on[y]) - query(in[y] - 1) << endl;
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    t = 1;
    //cin >> t;
    while (t--) {
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

