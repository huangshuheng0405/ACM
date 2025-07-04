# P1601 A+B Problem（高精）

## 题目描述

高精度加法，相当于 a+b problem，**不用考虑负数**。

## 输入格式

分两行输入。$a,b \leq 10^{500}$。

## 输出格式

输出只有一行，代表 $a+b$ 的值。

## 输入输出样例 #1

### 输入 #1

```
1
1
```

### 输出 #1

```
2
```

## 输入输出样例 #2

### 输入 #2

```
1001
9099
```

### 输出 #2

```
10100
```

## 说明/提示

$20\%$ 的测试数据，$0\le a,b \le10^9$；

$40\%$ 的测试数据，$0\le a,b \le10^{18}$。

# solve

最多比max(a,b)大1位 99+99  

删除前导零，要保留一位，因为有可能就是$0+0=0$

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    std::string a, b;
    std::cin >> a >> b;
    reverse(a.begin(), a.end());
    reverse(b.begin(), b.end());
    int n = std::max(a.size(), b.size()) + 1;
    std::vector<int> A(n + 1), C(n + 2), B(n + 1);
    for (int i = 0; i < a.size(); i++) {
        A[i + 1] = a[i] - '0';
    }
    for (int i = 0; i < b.size(); i++) {
        B[i + 1] = b[i] - '0';
    }
    for (int i = 1; i <= n; i++) {//核心
        C[i] += A[i] + B[i];
        C[i + 1] = C[i] / 10;
        C[i] %= 10;
    }
    while (C[n] == 0 && n > 1) {//去除前导0
        n--;
    }
    for (int i = n; i > 0; i--) {
        std::cout << C[i];
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

