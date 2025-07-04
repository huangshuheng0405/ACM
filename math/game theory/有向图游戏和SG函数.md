# 有向图游戏和 SG 函数

### 有向图游戏

给定一个**有向无环图**，图中只有一个起点，在起点上放一个棋子，两个玩家轮流沿着有向边推动棋子，每次走一步，不能走的玩家失败

### $mex$运算（minimum exclusion）

定义 mex 函数的值为不属于集合 S 中的最小非负整数，即：
$$
mex(S)=min\{x\}  (x\notin S,x\in N)
$$

例如$mex({0,2,4})=1,mex({1,2})=0$.

### $SG$函数

对于状态（节点） x 和它的所有 k 个后继状态（子节点）$y_1,y_2,\cdots,y_k$,定义 SG 函数:
$$
SG(x)=mex\{SG(y_1),SG(y_2,\cdots,SG(y_k)\}
$$

### $SG$定理

对于由 n 个有向图游戏组成的组合游戏，设他们的**起点**分别为$s_1,s_2,\cdots,s_n$,则有定理：当且仅当
$$
SG(x')=SG(s_1')\oplus SG(s_2')\oplus \cdots \oplus SG(s_n')\neq0
$$

这个时候，<u>游戏先手是必胜的，</u>同时，这是一个组合游戏的游戏状态 x 的 SG 值

这一定理被称为 Sprague—Grundy 定理，简称 SG 定理 

## 例题

给定一个有 N 个节点的有向无环图，图中某些节点上有棋子，两名玩家交替移动棋子。

玩家每一步可将任意一颗棋子沿一条有向边移动到另一个点，无法移动者输掉游戏。

对于给定的图和棋子初始位置，双方都会采取最优的行动，询问先手必胜还是先手必败。

第一行，三个整数 N , M, K，N 表示图中节点总数，M 表示图中边的条数，K 表示棋子的个数。

接下来 M 行，每行两个整数 X, Y 表示有一条边从 X 出发指向 Y。

接下来一行，K 个空格间隔的整数，表示初始时，棋子所在的节点编号。

若先手胜，输出 `win`，否则输出 `lose`。

## 题解

每个棋子都是孤立的，k个棋子拆分成k个有向图游戏，利用sg定理判断即可

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, m, k;
    cin >> n >> m >> k;
    vector<int> adj[n + 1];
    for (int i = 1; i <= m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
    }
    vector<int> f(n + 1, -1);
    function<int(int)> sg = [&](int u) {
        if (f[u] != -1) { // 记忆化搜索 保证每个节点只搜一次
            return f[u];
        }
        set<int> s;
        for (auto v : adj[u]) { // 把子节点的sg值插入当节点的集合
            s.insert(sg(v));
        }
        for (int i = 0;; i++) { // 找出当前节点的sg值
            if (!s.count(i)) {
                return f[u] = i;
            }
        }
    };
    int ans = 0;
    for (int i = 1; i <= k; i++) {
        int x;
        cin >> x;
        ans ^= sg(x);
    }
    if (ans) {
        cout << "win\n";
    } else {
        cout << "lose\n";
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

集合型Nim游戏

给定 𝑚 个整数组成的集合 $a_i$，给定 𝑛 堆石子的数量 $b_i$。
两位玩家轮流操作，每次操作可以从任意一堆石子中拿取石子，每次拿取的石子数量必须是集合 𝑎 中的整数，最后无法进行操作的人视为失败。
如果两人都采用最优策略，问先手是否必胜。

**思路**：每堆石子都是孤立的，把 𝑛 堆石子看做 𝑛 个有向图游戏

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, m;
    cin >> m;
    vector<int> a(m + 1);
    for (int i = 1; i <= m; i++) {
        cin >> a[i];
    }
    vector<int> f(n + 1, -1);
    function<int(int)> sg = [&](int u) {
        if (f[u] != -1) { // 记忆化搜索 保证每个节点只搜一次
            return f[u];
        }
        set<int> s;
        for (int i = 1; i <= m; i++) { // 把子节点的sg值插入当节点的集合
            if (u >= a[i]) {
                s.insert(sg(u - a[i]));
            }
        }
        for (int i = 0;; i++) { // 找出当前节点的sg值
            if (!s.count(i)) {
                return f[u] = i;
            }
        }
    };
    cin >> n;
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        ans ^= sg(x);
    }
    if (ans) {
        cout << "win\n";
    } else {
        cout << "lose\n";
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

