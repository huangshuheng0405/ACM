# 多重背包

与01背包的区别在于每种物品**有$k_i$个**，而非一个

一般做法：把每种物品选$k_i$次等价转化为有$k_i$个相同的物品，每个物品选一次，这样就转为01背包，状态转移方程如下
$$
f_{i,j}=max^{k_i}_{k=0}(f_{i-1},j-k\times w_i\times k)
$$

```c++
for(int i=1;i<=n;i++)//枚举物品
        for(int j=w;j>=w[i];j--)//枚举容量
            for(int k=1,k*w[i]<=j&&k<=cnt[i];k++)//枚举取的个数
                dp[j]=max(dp[j],dp[j-k*w[i]]+k*w[i]);
```

逐个枚举会超时，把大数量的物品用二进制拆分成$2^j$个单个物品而成的大物品，如果不是2的整数次幂最后还要把剩下的记上，把枚举的次数减少了就可以做01背包了

# 宝物筛选

## 题目描述

终于，破解了千年的难题。小 FF 找到了王室的宝物室，里面堆满了无数价值连城的宝物。

这下小 FF 可发财了，嘎嘎。但是这里的宝物实在是太多了，小 FF 的采集车似乎装不下那么多宝物。看来小 FF 只能含泪舍弃其中的一部分宝物了。

小 FF 对洞穴里的宝物进行了整理，他发现每样宝物都有一件或者多件。他粗略估算了下每样宝物的价值，之后开始了宝物筛选工作：小 FF 有一个最大载重为 $W$ 的采集车，洞穴里总共有 $n$ 种宝物，每种宝物的价值为 $v_i$，重量为 $w_i$，每种宝物有 $m_i$ 件。小 FF 希望在采集车不超载的前提下，选择一些宝物装进采集车，使得它们的价值和最大。

## 输入格式

第一行为一个整数 $n$ 和 $W$，分别表示宝物种数和采集车的最大载重。

接下来 $n$ 行每行三个整数 $v_i,w_i,m_i$。

## 输出格式

输出仅一个整数，表示在采集车不超载的情况下收集的宝物的最大价值。

## 样例 #1

### 样例输入 #1

```
4 20
3 9 3
5 9 1
9 4 2
8 1 3
```

### 样例输出 #1

```
47
```

## 提示

对于 $30\%$ 的数据，$n\leq \sum m_i\leq 10^4$，$0\le W\leq 10^3$。

对于 $100\%$ 的数据，$n\leq \sum m_i \leq 10^5$，$0\le W\leq 4\times 10^4$，$1\leq n\le 100$。

# 题解

### 二进制优化

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

constexpr int N = 1E5 + 5;

int vol[N], val[N], dp[40005];

void solve() {
    int n, w;
    cin >> n >> w;

    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        int v, w, s;
        cin >> v >> w >> s;

        int k = 1;
        while (k <= s) {
            val[++cnt] = k * v, vol[cnt] = k * w;
            s -= k;
            k *= 2;
        }
        if (s) {
            val[++cnt] = s * v, vol[cnt] = s * w;
        }
    }

    for (int i = 1; i <= cnt; i++) {
        for (int j = w; j >= vol[i]; j--) {
            dp[j] = max(dp[j], dp[j - vol[i]] + val[i]);
        }
    }

    cout << dp[w] << "\n";
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

### 单调队列优化