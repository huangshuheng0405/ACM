# 分组背包

就是从所有物品中选择一件，变成了**从当前组中选择一件**，于是就对每一组进行一次01背包即可

# 通天之分组背包

## 题目背景

直达通天路·小 A 历险记第二篇

## 题目描述

自 $01$ 背包问世之后，小 A 对此深感兴趣。一天，小 A 去远游，却发现他的背包不同于 $01$ 背包，他的物品大致可分为 $k$ 组，每组中的物品相互冲突，现在，他想知道最大的利用价值是多少。

## 输入格式

两个数 $m,n$，表示一共有 $n$ 件物品，总重量为 $m$。

接下来 $n$ 行，每行 $3$ 个数 $a_i,b_i,c_i$，表示物品的重量，利用价值，所属组数。

## 输出格式

一个数，最大的利用价值。

## 样例 #1

### 样例输入 #1

```
45 3
10 10 1
10 5 1
50 400 2
```

### 样例输出 #1

```
10
```

## 提示

$0 \leq m \leq 1000$，$1 \leq n \leq 1000$，$1\leq k\leq 100$，$a_i, b_i, c_i$ 在 `int` 范围内。

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int vol[1005], val[1005], dp[1005], cnt[1005], g[105][1005];

void solve() {
    int n, m;
    cin >> m >> n;

    int mx = 0;
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> vol[i] >> val[i] >> x;
        mx = max(mx, x);  // mx记录最大组数
        cnt[x]++;         // 记录第i组有几个物品
        g[x][cnt[x]] = i; // 记录第i组第cnt[x]个物品的编号
    }

    for (int i = 1; i <= mx; i++) {
        for (int j = m; j >= 0; j--) {
            for (int k = 1; k <= cnt[i]; k++) {
                if (j >= vol[g[i][k]]) {
                    dp[j] = max(dp[j], dp[j - vol[g[i][k]]] + val[g[i][k]]);
                }
            }
        }
    }

    cout << dp[m] << "\n";
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

