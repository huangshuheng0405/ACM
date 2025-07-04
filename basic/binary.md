# binary search

## 二分查找

```c++
//   二分算法（整数域）： 前驱

int lo = 1, hi = 1E9;
while (lo < hi) {
    int m = (lo + hi + 1) / 2;
    if (check(m)) {
        lo = m;
    } else {
        hi = m - 1;
    }
}
std::cout << lo << "\n";

//二分算法（整数域）：后继

int lo = 1, hi = n;
while (lo < hi) {
    int m = (lo + hi) / 2;
    if (check(m)) {
        hi = m;
    } else {
        lo = m + 1;
    }
}
std::cout << lo << "\n";
```



# P2249 【深基13.例1】查找

## 题目描述

输入 $n$ 个不超过 $10^9$ 的单调不减的（就是后面的数字不小于前面的数字）非负整数 $a_1,a_2,\dots,a_{n}$，然后进行 $m$ 次询问。对于每次询问，给出一个整数 $q$，要求输出这个数字在序列中第一次出现的编号，如果没有找到的话输出 $-1$ 。

## 输入格式

第一行 $2$ 个整数 $n$ 和 $m$，表示数字个数和询问次数。

第二行 $n$ 个整数，表示这些待查询的数字。

第三行 $m$ 个整数，表示询问这些数字的编号，从 $1$ 开始编号。

## 输出格式

输出一行，$m$ 个整数，以空格隔开，表示答案。

## 输入输出样例 #1

### 输入 #1

```
11 3
1 3 3 3 5 7 9 11 13 15 15
1 3 6
```

### 输出 #1

```
1 2 -1
```

## 说明/提示

数据保证，$1 \leq n \leq 10^6$，$0 \leq a_i,q \leq 10^9$，$1 \leq m \leq 10^5$

本题输入输出量较大，请使用较快的 IO 方式。

# Solve

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n, m;
    std::cin >> n >> m;

    std::vector<int> a(n);
    for (int i = 0; i < n; i++) {
        std::cin >> a[i];
    }

    while (m--) {
        int x;
        std::cin >> x;

        int L = 0, R = n - 1;
        while (L < R) {
            int mid = (L + R) >> 1;
            if (a[mid] >= x) {
                R = mid;
            } else {
                L = mid + 1;
            }
        }

        if (a[L] != x) {
            std::cout << "-1 ";
        } else {
            std::cout << L + 1 << " ";
        }
    }
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

# 三分

如果需要求出单峰函数的极值点，通常使用二分法衍生出的三分法求单峰函数的极值点

三分法与二分法的基本思想类似，但每次操作需在当前区间$[l.r]$（下图中除去虚线范围内的部分）内任取两点 $lmid,rmid(lmid)$（下图中的两蓝点）。如下图，如果 $f(lmid)<f(rmid)$，则在 $[rmid,r]$（下图中的红色部分）中函数必然单调递增，最小值所在点（下图中的绿点)必然不在这一区间内，可舍去这一区间。反之亦然。

![](https://oi-wiki.org/basic/images/binary1.svg)

> 在计算$lmid$和$rmid$时，需要防止数据溢出的现象出现。

### 模板（三分答案）

```c++
//check函数根据题目要求写
while(l <= r) {
	int midl = l + (r - l) / 3;
	int midr = r - (r - l) / 3;
	if(check(midl) <= check(midr)) r = midr - 1;
	else l = midl + 1;
}
res = min(check(r), check(l));

```

# Acwing 5201 午餐音乐会

[题面](https://www.acwing.com/problem/content/description/5204/)

一维数轴上站着 $N$ 个人，编号 $1 \sim N$。

初始时，第 $i$ 个人位于整数坐标位置 $P_i$，此人移动 $1$ 单位距离所需的成本为 $W_i$，他能听到与他相距不超过 $D_i$ 的所有位置发出的声音。

不同的人的位置可以重叠。

现在，我们需要选择一个整数坐标位置，并在此位置举办一场音乐会。

没有人想要错过这场音乐会，所以音乐会开始后，所有听不到音乐的人都会朝音乐会举办位置方向移动，直到移动至可以听到音乐的位置为止。

我们希望合理选择音乐会的举办位置，使得所有人的移动总成本尽可能小。

请你输出这个总成本的最小可能值。

#### 输入格式

第一行包含一个整数 $N$。

接下来 $N$ 行，每行包含三个整数 $P_i,W_i,D_i$。

#### 输出格式

一个整数，表示最小总成本。

#### 数据范围

$1 \le N \le 2 \times 10^5$,  
$0 \le P_i \le 10^9$,  
$1 \le W_i \le 1000$,  
$0 \le D_i \le 10^9$

#### 输入样例1：

```
1
0 1000 0
```

#### 输出样例1：

```
0
```

#### 样例1解释

最佳方案是在位置 $0$ 举办音乐会，这样唯一的人无需任何移动即可听到音乐会。

#### 输入样例2：

```
2
10 4 3
20 4 2
```

#### 输出样例2：

```
20
```

#### 样例2解释

一种最佳方案是在位置 $14$ 举办音乐会，这样的话，第一个人需要移动至位置 $11$，所需成本为 $(11-10) \times 4 = 4$，第二个人需要移动至位置 $16$，所需成本为 $(20-16) \times 4 = 16$，总成本为 $4+16=20$。

#### 输入样例3：

```
3
6 8 3
1 4 1
14 5 2
```

#### 输出样例3：

```
43
```

# Solve

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n;
    std::cin >> n;

    std::vector<std::array<int, 3>> a(n);
    for (int i = 0; i < n; i++) {
        int p, w, d;
        std::cin >> p >> w >> d;

        a[i] = {p, w, d};
    }

    auto check = [&](int mid) {
        i64 res = 0;
        for (int i = 0; i < n; i++) {
            auto [p, w, d] = a[i];
            if (mid >= p - d && mid <= p + d) {//在区域内不需要移动
                continue;
            }
            if (mid < p) {//计算成本
                res += (i64)(p - d - mid) * w;
            } else {
                res += (i64)(mid - p - d) * w;
            }
        }
        return res;
    };

    int L = 0, R = 1E9;
    while (L <= R) {
        int midL = L + (R - L) / 3;
        int midR = R - (R - L) / 3;
        if (check(midL) <= check(midR)) {
            R = midR - 1;
        } else {
            L = midL + 1;
        }
    }

    std::cout << std::min(check(R), check(L)) << "\n";
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

