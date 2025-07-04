# Hermite

# B2146 Hermite 多项式

## 题目描述

求 Hermite 多项式的值。

$$
h_n(x)=\left\{
\begin{aligned}
1&，&n=0\\
2x&，&n=1\\
2xh_{n-1}(x)-2(n-1)h_{n-2}(x)&，& n>1
\end{aligned}
\right.
$$



对给定的 $x$ 和正整数 $n$，求多项式的值。

## 输入格式

输入 $n$ 和 $x$ 。

## 输出格式

求多项式的值。

## 输入输出样例 #1

### 输入 #1

```
1 2
```

### 输出 #1

```
4
```

## 说明/提示

$(n \le 8，x \le 8)$

# Solve

`cmath`库中自带的库函数`hermite`

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n, x;
    std::cin >> n >> x;

    std::cout << std::hermite(n, x) << "\n";
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

