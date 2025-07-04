# [USACO06NOV] Corn Fields G

## 题目描述

Farmer John has purchased a lush new rectangular pasture composed of M by N (1 ≤ M ≤ 12, 1 ≤ N ≤ 12) square parcels. He wants to grow some yummy corn for the cows on a number of squares. Regrettably, some of the squares are infertile and can't be planted. Canny FJ knows that the cows dislike eating close to each other, so when choosing which squares to plant, he avoids choosing squares that are adjacent; no two chosen squares share an edge. He has not yet made the final choice as to which squares to plant.

Being a very open-minded man, Farmer John wants to consider all possible options for how to choose the squares for planting. He is so open-minded that he considers choosing no squares as a valid option! Please help Farmer John determine the number of ways he can choose the squares to plant.

农场主 $\rm John$ 新买了一块长方形的新牧场，这块牧场被划分成 $M$ 行 $N$ 列 $(1 \le M \le 12, 1 \le  N \le 12)$，每一格都是一块正方形的土地。 $\rm John$ 打算在牧场上的某几格里种上美味的草，供他的奶牛们享用。

遗憾的是，有些土地相当贫瘠，不能用来种草。并且，奶牛们喜欢独占一块草地的感觉，于是 $\rm John$ 不会选择两块相邻的土地，也就是说，没有哪两块草地有公共边。

$\rm John$ 想知道，如果不考虑草地的总块数，那么，一共有多少种种植方案可供他选择？（当然，把新牧场完全荒废也是一种方案）

## 输入格式

第一行：两个整数 $M$ 和 $N$，用空格隔开。

第 $2$ 到第 $M+1$ 行：每行包含 $N$ 个用空格隔开的整数，描述了每块土地的状态。第 $i+1$ 行描述了第 $i$ 行的土地，所有整数均为 $0$ 或 $1$ ，是 $1$ 的话，表示这块土地足够肥沃，$0$ 则表示这块土地不适合种草。

## 输出格式

一个整数，即牧场分配总方案数除以 $100,000,000$ 的余数。

## 样例 #1

### 样例输入 #1

```
2 3
1 1 1
0 1 0
```

### 样例输出 #1

```
9
```

# 题解

1. 行内合法：如果$!(i\&i>>1)$为真，则$i$合法
2. 行间兼容：如果$!(a\&b)\&\&(a\&g[i])==a$为真，则$a,b$兼容,后面条件保证合法状态符合地图种植条件
3. 状态：$dp[i],a$表示已经种植前$i$，第$i$行第$a$个状态时的方案数
4. 状态转移：$dp[i,a]=\sum dp[i-1,b]$
5. 方案总数：$ans=\sum dp[n,a]$

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int g[14], s[1 << 14], dp[14][1 << 14];

constexpr int mod = 1E8;

void solve() {
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            int x;
            cin >> x;

            g[i] = (g[i] << 1) + x; // 保存各行的状态值
        }
    }

    int cnt = 0;
    for (int i = 0; i < (1 << m); i++) { // 枚举一行中所有状态
        if (!(i & i >> 1)) {             // 行内不存在相邻
            s[cnt++] = i;
        }
    }

    dp[0][0] = 1;
    for (int i = 1; i <= n + 1; i++) {
        for (int a = 0; a < cnt; a++) {
            for (int b = 0; b < cnt; b++) {
                if (!(s[a] & s[b]) && (s[a] & g[i]) == s[a]) {
                    dp[i][a] = (dp[i][a] + dp[i - 1][b]) % mod;
                }
            }
        }
    }

    cout << dp[n + 1][0] << "\n";
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

