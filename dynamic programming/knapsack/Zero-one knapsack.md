# 01背包

# [NOIP2005 普及组] 采药

## 题目描述

辰辰是个天资聪颖的孩子，他的梦想是成为世界上最伟大的医师。为此，他想拜附近最有威望的医师为师。医师为了判断他的资质，给他出了一个难题。医师把他带到一个到处都是草药的山洞里对他说：“孩子，这个山洞里有一些不同的草药，采每一株都需要一些时间，每一株也有它自身的价值。我会给你一段时间，在这段时间里，你可以采到一些草药。如果你是一个聪明的孩子，你应该可以让采到的草药的总价值最大。”


如果你是辰辰，你能完成这个任务吗？

## 输入格式

第一行有 $2$ 个整数 $T$（$1 \le T \le 1000$）和 $M$（$1 \le  M \le 100$），用一个空格隔开，$T$ 代表总共能够用来采药的时间，$M$ 代表山洞里的草药的数目。

接下来的 $M$ 行每行包括两个在 $1$ 到 $100$ 之间（包括 $1$ 和 $100$）的整数，分别表示采摘某株草药的时间和这株草药的价值。

## 输出格式

输出在规定的时间内可以采到的草药的最大总价值。

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
3
```

## 提示

**【数据范围】**

- 对于 $30\%$ 的数据，$M \le 10$；
- 对于全部的数据，$M \le 100$。

**【题目来源】**

NOIP 2005 普及组第三题

# 题解

**状态**：$dp[i][j]$表示前$i$件物品放入容量为$j$的背包的最大价值，$dp[n][m]$就是最终答案

$w[i]$表示第$i$件物品的体积，$v[i]$表示第$i$件物品的价值

### 状态转移方程

如果当前背包容量$j\leq w[i]$，不能放入，则$dp[i][j]=dp[i-1][j]$

如果当前背包容量$j\geq w[i]$，能放入，但要比较代价，结合两种情况得到下列转移方程
$$
\large dp_{i,j}=max(dp_{i-1,j},dp_{i-1,j-w_i}+v_i)
$$

如果直接用二维数组记录状态会MLE，可以用滚动数组的形式来优化，因为对$f_i$有影响的只有$f_{i-1}$，可以去掉第一维直接用$f_i$来表示处理到当前物品时背包容量为i的最大价值，可以得出以下方程
$$
\large dp_j=max(dp_j,dp_{j-w_i}+v_i)
$$

背包容量从小往大枚举会出错，因为$dp[j-w[i]]$（旧值）先于$dp[j]$（新值）更新，用$dp[j-w[i]]$去更新$dp[j]$，会出错

所以背包容量要**逆序枚举**

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <cstring>
#include <cmath>
#include <set>
#include <map>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1}; // 上右下左

void solve()
{
    int t, m;
    cin >> t >> m;
    vector<int> val(m + 1), vol(m + 1);
    for (int i = 1; i <= m; i++)
    {
        cin >> vol[i] >> val[i];
    }
    vector<int> dp(t + 1);
    for (int i = 1; i <= m; i++) // 枚举每个物品
    {
        for (int j = t; j >= vol[i]; j--) // 注意枚举顺序
        {
            dp[j] = max(dp[j], dp[j - vol[i]] + val[i]);
        }
    }
    cout << dp[t];
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;
    // cin >> _;
    while (_--)
        solve();
    return 0;
}

```

# Knapsack 2

## 题面翻译

$N$ 个物品被编号为 $1, 2, \ldots, N$。对于 $1 \leq i \leq N$，物品 $i$ 的重量是 $w _ i$，价值是 $v _ i$。

太郎君决定从 $N$ 个物品中选择一些放入背包中带回家。背包的容量为 $W$，带回的物品的总重量不能超过 $W$。

请计算太郎君能带回的物品的最大总价值。

### 输入格式

输入以以下格式从标准输入中提供：

> $N$ $W$  
> $w _ 1$ $v _ 1$  
> $w _ 2$ $v _ 2$  
> $\ldots$  
> $w _ N$ $v _ N$

### 输出格式

输出太郎君能带回的物品的最大总价值。

### 限制条件

- 所有输入均为整数。
- $1 \leq N \leq 100$
- $1 \leq W \leq 10 ^ 9$
- $1 \leq w _ i \leq W$
- $1 \leq v _ i \leq 10 ^ 3$

### 样例解释 1

可以选择物品 $1$ 和 $3$。这样，总重量为 $3 + 5 = 8$，总价值为 $30 + 60 = 90$。

### 样例解释 3

可以选择物品 $2, 4, 5$。这样，总重量为 $5 + 6 + 3 = 14$，总价值为 $6 + 6 + 5 = 17$。

---

Translated by User 735713.

## 题目描述

[problemUrl]: https://atcoder.jp/contests/dp/tasks/dp_e

$ N $ 個の品物があります。 品物には $ 1,\ 2,\ \ldots,\ N $ と番号が振られています。 各 $ i $ ($ 1\ \leq\ i\ \leq\ N $) について、品物 $ i $ の重さは $ w_i $ で、価値は $ v_i $ です。

太郎君は、$ N $ 個の品物のうちいくつかを選び、ナップサックに入れて持ち帰ることにしました。 ナップサックの容量は $ W $ であり、持ち帰る品物の重さの総和は $ W $ 以下でなければなりません。

太郎君が持ち帰る品物の価値の総和の最大値を求めてください。

## 输入格式

入力は以下の形式で標準入力から与えられる。

> $ N $ $ W $ $ w_1 $ $ v_1 $ $ w_2 $ $ v_2 $ $ : $ $ w_N $ $ v_N $

## 输出格式

太郎君が持ち帰る品物の価値の総和の最大値を出力せよ。

## 样例 #1

### 样例输入 #1

```
3 8
3 30
4 50
5 60
```

### 样例输出 #1

```
90
```

## 样例 #2

### 样例输入 #2

```
1 1000000000
1000000000 10
```

### 样例输出 #2

```
10
```

## 样例 #3

### 样例输入 #3

```
6 15
6 5
5 6
6 4
6 6
3 5
7 2
```

### 样例输出 #3

```
17
```

## 提示

### 制約

- 入力はすべて整数である。
- $ 1\ \leq\ N\ \leq\ 100 $
- $ 1\ \leq\ W\ \leq\ 10^9 $
- $ 1\ \leq\ w_i\ \leq\ W $
- $ 1\ \leq\ v_i\ \leq\ 10^3 $

### Sample Explanation 1

品物 $ 1,\ 3 $ を選べばよいです。 すると、重さの総和は $ 3\ +\ 5\ =\ 8 $ となり、価値の総和は $ 30\ +\ 60\ =\ 90 $ となります。

### Sample Explanation 3

品物 $ 2,\ 4,\ 5 $ を選べばよいです。 すると、重さの総和は $ 5\ +\ 6\ +\ 3\ =\ 14 $ となり、価値の総和は $ 6\ +\ 6\ +\ 5\ =\ 17 $ となります。

# 题解

此题背包容量$10^9$，普通01背包做法TLE，但是价值只有$10^3$

### 状态

$dp[i][j]$，对于前$i$件物品价值$j$的最小重量

### 状态转移方程

$$
\large dp[i][j]=min(dp[i-1][j],dp[i-1][j-val[i]]+vol[i])
$$

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

constexpr int N = 1e6 + 5;
;

i64 vol[105], val[105], dp[N];

void solve() { // 从枚举容量转换成枚举价值
    int n, w;
    cin >> n >> w;

    int mx = n * 1000; // 物品数*每件物品最大价值
    memset(dp, 0x3f, sizeof(dp));
    dp[0] = 0;

    for (int i = 1; i <= n; i++) {
        cin >> vol[i] >> val[i];
    }

    for (int i = 1; i <= n; i++) {
        for (int j = mx; j >= val[i]; j--) {
            dp[j] = min(dp[j], dp[j - val[i]] + vol[i]);
        }
    }

    for (int i = mx; i >= 0; i--) {
        if (dp[i] <= w) {      // 满足重量条件输出
            cout << i << "\n"; // 最大价值
            return;
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

# 恰好装满问题

你有一个背包，最多能容纳的体积是V。

现在有n个物品，第i个物品的体积为$v_i$ ,价值为$w_i$。

（1）求这个背包至多能装多大价值的物品？

（2）若背包**恰好装满**，求至多能装多大价值的物品？

# 题解

第一问是普通背包，第二问，把$dp$数组初始化为负无穷大，由$j-val[i]$来更新，记得初始化$dp[0]=0$

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
        for (int j = v; j >= vol[i]; j--) {
            dp[j] = max(dp[j], dp[j - vol[i]] + val[i]);
        }
    }

    cout << dp[v] << "\n";

    memset(dp, -0x3f, sizeof(dp));
    dp[0] = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = v; j >= vol[i]; j--) {
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

# P1060 [NOIP 2006 普及组] 开心的金明

## 题目描述

金明今天很开心，家里购置的新房就要领钥匙了，新房里有一间他自己专用的很宽敞的房间。更让他高兴的是，妈妈昨天对他说：“你的房间需要购买哪些物品，怎么布置，你说了算，只要不超过 $N$ 元钱就行”。今天一早金明就开始做预算，但是他想买的东西太多了，肯定会超过妈妈限定的 $N$ 元。于是，他把每件物品规定了一个重要度，分为 $5$ 等：用整数 $1-5$ 表示，第 $5$ 等最重要。他还从因特网上查到了每件物品的价格（都是整数元）。他希望在不超过 $N$ 元（可以等于 $N$ 元）的前提下，使每件物品的价格与重要度的乘积的总和最大。

设第$j$件物品的价格为 $v_j$，重要度为 $w_j$，共选中了 $k$ 件物品，编号依次为 $j_1,j_2,…,j_k$，则所求的总和为：

$$v_{j_1} \times w_{j_1}+v_{j_2} \times w_{j_2} …+v_{j_k} \times w_{j_k}$$

请你帮助金明设计一个满足要求的购物单。

## 输入格式

第一行，为 $2$ 个正整数，用一个空格隔开：$n,m$（$n<30000,m<25$）其中 $n$ 表示总钱数，$m$ 为希望购买物品的个数。

从第 $2$ 行到第 $m+1$ 行，第 $j$ 行给出了编号为 $j-1$ 的物品的基本数据，每行有 $2$ 个非负整数 $v,p$（其中 $v$ 表示该物品的价格 $(v \le 10000)$，$p$ 表示该物品的重要度（$1\le p\le5$）。

## 输出格式

$1$ 个正整数，为不超过总钱数的物品的价格与重要度乘积的总和的最大值（$<100000000$）。

## 输入输出样例 #1

### 输入 #1

```
1000 5
800 2
400 5
300 5
400 3
200 2
```

### 输出 #1

```
3900
```

## 说明/提示

NOIP 2006 普及组 第二题

# solve

01背包

```c++
#include<bits/stdc++.h>

using i64 = long long;
using ld = long double;

int main () {
	std::ios::sync_with_stdio(false);	
	std::cin.tie(nullptr);

	int n, m;
	std::cin >> n >> m;

	std::vector<int> v(m), p(m);
	for (int i = 0; i < m; i++) {
		std::cin >> v[i] >> p[i];
		p[i] *= v[i];// p数组 存重要度和价钱乘积
	}

	std::vector<int> dp(n + 1);
	for (int i = 0; i < m; i++) {// 枚举物品个数
		for (int j = n; j >= v[i]; j--) {// 逆序枚举容量
			dp[j] = std::max(dp[j], dp[j - v[i]] + p[i]);
		}
	}

	std::cout << dp[n] << "\n";

	return 0;
}
```

