# 前缀和

对于一个序列$a$，它的前缀和数组$sum$有
$$
sum[1]=a[1],sum[i]=sum[i-1]+a[i],(2\leq i \leq n)
$$
求区间和$[l,r]$时间复杂度$O(1)$
$$
sum[r]-sum[l-1]
$$


# 【深进1.例1】求区间和

## 题目描述

给定 $n$ 个正整数组成的数列 $a_1, a_2, \cdots, a_n$ 和 $m$ 个区间 $[l_i,r_i]$，分别求这 $m$ 个区间的区间和。

## 输入格式

第一行，为一个正整数 $n$ 。

第二行，为 $n$ 个正整数 $a_1,a_2, \cdots ,a_n$

第三行，为一个正整数 $m$ 。

接下来 $m$ 行，每行为两个正整数 $l_i,r_i$ ，满足 $1\le l_i\le r_i\le n$

## 输出格式

共 $m$ 行。

第 $i$ 行为第 $i$ 组答案的询问。

## 样例 #1

### 样例输入 #1

```
4
4 3 2 1
2
1 4
2 3
```

### 样例输出 #1

```
10
5
```

## 提示

样例解释：第 1 到第 4 个数加起来和为 10。第 2 个数到第 3 个数加起来和为 5。

对于 $50\%$ 的数据：$n,m\le 1000$ ；

对于 $100\%$ 的数据：$1 \leq n,m\le 10^5$，$1 \leq a_i\le 10^4$。

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
const int N = 2e5 + 5, mod = 1e9 + 7;
int t, n, a[N], b[N], q, l, r;

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        b[i] = b[i - 1] + a[i];//计算前缀和
    }
    cin >> q;
    while (q--) {
        cin >> l >> r;
        cout << b[r] - b[l - 1] << endl;//记住l要减1
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

# 二维前缀和

递推求前缀和
$$
\large sum_{i,j}=sum_{i-1,j}+sum_{i,j-1}-sum_{i-1,j-1}+a_{i,j}
$$

因为同时加了$sum_{i-1,j}$和$sum_{i,j-1}$，重复多加了一次$sum_{i-1,j-1}$，所以要减去（基于容斥原理）

求区间和：$x1,x2$到$x2,y2$
$$
\large sum_{x_1y_1,x_2,y_2}=sum_{x_2,y_2}-sum_{x_1-1,y_2}-sum_{x_2,y_1-1}+sum_{x_1-1,y_1-1}
$$

# [HNOI2003] 激光炸弹

## 题目描述

一种新型的激光炸弹，可以摧毁一个边长为 $m$ 的正方形内的所有目标。现在地图上有 $n$ 个目标，用整数 $x_i$ , $y_i$ 表示目标在地图上的位置，每个目标都有一个价值 $v_i$。激光炸弹的投放是通过卫星定位的，但其有一个缺点，就是其爆破范围，即那个边长为 $m$ 的边必须与 $x$ 轴，$y$ 轴平行。若目标位于爆破正方形的边上，该目标不会被摧毁。

现在你的任务是计算一颗炸弹最多能炸掉地图上总价值为多少的目标。

可能存在多个目标在同一位置上的情况。

## 输入格式

输入的第一行为整数 $n$ 和整数 $m$；

接下来的 $n$ 行，每行有 $3$ 个整数 $x, y, v$，表示一个目标的坐标与价值。

## 输出格式

输出仅有一个正整数，表示一颗炸弹最多能炸掉地图上总价值为多少的目标（结果不会超过 $32767$ ）。

## 样例 #1

### 样例输入 #1

```
2 1
0 0 1
1 1 1
```

### 样例输出 #1

```
1
```

## 提示

**数据规模与约定**

- 对于 $100\%$ 的数据，保证 $1 \le n \le 10^4$，$0 \le x_i ,y_i \le 5\times 10^3$，$1 \le m \le 5\times 10^3$，$1 \le v_i < 100$。

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> sum(5007, vector<int>(5007, 0));
    for (int i = 0; i < n; i++) {
        int x, y, v;
        cin >> x >> y >> v;

        sum[x + 1][y + 1] += v;
    }

    for (int i = 1; i <= 5001; i++) {
        for (int j = 1; j <= 5001; j++) {
            sum[i][j] += sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1];
        }
    }

    int ans = 0;
    for (int i = m; i <= 5001; i++) {
        for (int j = m; j <= 5001; j++) {
            ans = max(ans, sum[i][j] - sum[i - m][j] - sum[i][j - m] +
                               sum[i - m][j - m]);
        }
    }

    cout << ans << "\n";
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

