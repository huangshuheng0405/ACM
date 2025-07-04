# 完全背包

01背包每个物品只能取一次，而完全背包每个物品能取**无限次**

# 疯狂的采药

## 题目背景

此题为纪念 LiYuxiang 而生。

## 题目描述

LiYuxiang 是个天资聪颖的孩子，他的梦想是成为世界上最伟大的医师。为此，他想拜附近最有威望的医师为师。医师为了判断他的资质，给他出了一个难题。医师把他带到一个到处都是草药的山洞里对他说：“孩子，这个山洞里有一些不同种类的草药，采每一种都需要一些时间，每一种也有它自身的价值。我会给你一段时间，在这段时间里，你可以采到一些草药。如果你是一个聪明的孩子，你应该可以让采到的草药的总价值最大。”

如果你是 LiYuxiang，你能完成这个任务吗？

此题和原题的不同点：

$1$. 每种草药可以无限制地疯狂采摘。

$2$. 药的种类眼花缭乱，采药时间好长好长啊！师傅等得菊花都谢了！

## 输入格式

输入第一行有两个整数，分别代表总共能够用来采药的时间 $t$ 和代表山洞里的草药的数目 $m$。

第 $2$ 到第 $(m + 1)$ 行，每行两个整数，第 $(i + 1)$ 行的整数 $a_i, b_i$ 分别表示采摘第 $i$ 种草药的时间和该草药的价值。

## 输出格式

输出一行，这一行只包含一个整数，表示在规定的时间内，可以采到的草药的最大总价值。

## 样例 #1

### 样例输入 #1

```
70 3
71 100
69 1
1 2
```

### 样例输出 #1

```
140
```

## 提示

#### 数据规模与约定

- 对于 $30\%$ 的数据，保证 $m \le 10^3$ 。
- 对于 $100\%$ 的数据，保证 $1 \leq m \le 10^4$，$1 \leq t \leq 10^7$，且 $1 \leq m \times t \leq 10^7$，$1 \leq a_i, b_i \leq 10^4$。

# 题解

### 状态

$dp[i][j]$表示前$i$件物品放入容量为$j$的背包的最大价值

如果当前背包容量$j\leq w[i]$或者能放入但不如不放，则有$dp[i][j]=dp[i-1][j]$

否则有$dp[i][j]=dp[i][j-w[i]]+val[i]$

对于前$i$件物品，背包容量为$j-w[i]$时可能已经放入了第$i$件物品，容量为$j$时还可以再放入第$i$件物品，所以用$dp[i][j-w[i]]$更新$dp[i][j]$

背包容量从**小往大推**，因为后面的答案是根据前面推的（跟01背包恰恰相反），在同一层进行状态转移，01背包是跟前一层的状态比较

转移从第一层开始往下叠加，跟层数没关系所以可以把第一维省掉

```c++
#include<bits/stdc++.h>

using i64 = long long;

void solve() {
    int t, m;
    std::cin >> t >> m;
    std::vector<i64>dp(t + 1), vol(m), val(m);
    for (int i = 0; i < m; i++) {
        std::cin >> vol[i] >> val[i];
    }
    for (int i = 0; i < m; i++) {
        for (int j = vol[i]; j <= t; j++) {
            dp[j] = std::max(dp[j], dp[j - vol[i]] + val[i]);
        }
    }
    std::cout << dp[t] << "\n";
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);
    int t = 1;
    // std::cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

# 恰好装满问题

你有一个背包，最多能容纳的体积是V。

现在有n种物品，每种物品有任意多个，第i种物品的体积为$v_i$ ,价值为$w_i$。  

（1）求这个背包至多能装多大价值的物品？

（2）若背包恰好装满，求至多能装多大价值的物品？

# 题解

与01背包问题相同，枚举容量时从小到大枚举即可

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int vol[1005], val[1005], dp[1005];

void solve() {
    int n, v;
    cin >> n >> v;

    for (int i = 1; i <= n; i++) {
        cin >> vol[i] >> val[i];
    }

    for (int i = 1; i <= n; i++) {
        for (int j = vol[i]; j <= v; j++) {
            dp[j] = max(dp[j], dp[j - vol[i]] + val[i]);
        }
    }

    cout << dp[v] << "\n";

    memset(dp, -0x3f, sizeof(dp));
    dp[0] = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = vol[i]; j <= v; j++) {
            dp[j] = max(dp[j], dp[j - vol[i]] + val[i]);
        }
    }

    if (dp[v] < 0) {
        dp[v] = 0;
    }

    cout << dp[v] << "\n";
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

