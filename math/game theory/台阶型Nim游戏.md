# 台阶型Nim游戏

[题面](http://poj.org/problem?id=1704)

给你一个单行网格的棋盘，给定 𝑛 个棋子在网格中的初始位置 𝑎𝑖。
每次玩家选择一个棋子，并将其向左移动，但是不能越过任何其他棋子或超过左边界。玩家可以自由选择棋子移动的步数，其限制是棋子必须至少移动一步，一个网格最多可以包含一个棋子。
无法移动任何棋子的玩家将输掉游戏。每次都由格鲁吉亚先手。请你预测谁将获得胜利。

**思路**：
转化为台阶型 Nim游戏：将空白格子看做台阶上的石子，棋子左移等价于空白格子右移，即石子向右移下台阶。所以，将 n-1,n 棋子间空白格子数记为 b1，将 n-2,n-1 棋子间空白格子数记为 b2，…，将 1 棋子前面的空白格子数记为 bn。
b1∼n分别代表第 1∼n 阶梯上摆放的石子数，$b_1\oplus b_3 \oplus b_5\dots \neq0$ 时，先手必胜。

```c++
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n + 1), b(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    sort(a.begin() + 1, a.end());
    for (int i = n; i >= 1; i--) {
        b[n - i + 1] = a[i] - a[i - 1] - 1;
    }
    int ans = 0;
    for (int i = 1; i <= n; i += 2) {
        ans ^= b[i];
    }
    if (ans) {
        cout << "Georgia will win\n";
    } else {
        cout << "Bob will win\n";
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int _ = 1;

    cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

# 树形博弈

[题面](https://acm.hdu.edu.cn/showproblem.php?pid=5996)

把所有奇数层的值异或，看是否为0

## 题解

```c++
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

using namespace std;

vector<int> adj[100005];
int dep[100005], a[100005];

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        adj[i].clear();
        dep[i] = 0;
    }
    for (int i = 1; i < n; i++) {
        int v;
        scanf("%d", &v);
        adj[v].push_back(i);
    }
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    int ans = 0;
    function<void(int, int)> dfs = [&](int u, int d) {
        if (d & 1) {
            ans ^= a[u];
        }
        for (auto v : adj[u]) {
            dep[v] = dep[u] + 1;
            dfs(v, dep[v]);
        }
    };
    dfs(0, 0);
    if (ans) {
        cout << "win\n";
    } else {
        cout << "lose\n";
    }
}
int main() {
    int _ = 1;

    cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

