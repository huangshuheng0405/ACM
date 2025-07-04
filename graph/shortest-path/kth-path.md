# K短路（A*算法）

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
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//上右下左
struct node {
    ll s, v, d;
    bool operator<(const node &x) const {
        return s > x.s;//从小打大排
    }
};

void solve() {
    int n, m, k;
    cin >> n >> m >> k;
    vector<pair<ll, ll>> adj1[n + 5], adj2[n + 5];
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        adj1[u].push_back({v, w});//正向图
        adj2[v].push_back({u, w});//反向图
    }
    int s, t;
    s = 1, t = n;
    vector<ll> dis(n + 5, inf);
    dis[t] = 0;
    vector<bool> vis(n + 5, false);
    priority_queue<pair<ll, ll>> q;
    q.push({0, t});
    while (!q.empty()) {//先反向跑一边
        int u = q.top().second;
        q.pop();
        if (vis[u] == true) {
            continue;
        }
        vis[u] = true;
        for (int i = 0; i < adj2[u].size(); i++) {
            ll v = adj2[u][i].first, w = adj2[u][i].second;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                q.push({-dis[v], v});
            }
        }
    }
    vector<ll> ans;
    for (int j = 1; j <= k; j++) {//再正向跑一遍
        vector<int> cnt(n + 5, 0);
        priority_queue<node> q1;
        q1.push({dis[s], s, 0});
        while (!q1.empty()) {
            ll u = q1.top().v, d1 = q1.top().d;
            q1.pop();
            cnt[u]++;
            if (cnt[t] == j) {
                ans.push_back(d1);
                break;
            }
            for (int i = 0; i < adj1[u].size(); i++) {
                ll v = adj1[u][i].first, w = adj1[u][i].second;
                w += d1;
                if (cnt[v] < k) {
                    q1.push({w + dis[v], v, w});
                }
            }
        }
    }
    for (int i = 0; i < k; i++) {
        cout << ans[i] << ' ';
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--)
        solve();
    return 0;
}
```

