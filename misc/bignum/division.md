# 高精度除法

# A/B Problem

## 题目描述

输入两个整数 $a,b$，输出它们的商。

## 输入格式

两行，第一行是被除数，第二行是除数。

## 输出格式

一行，商的整数部分。

## 样例 #1

### 样例输入 #1

```
10
2
```

### 样例输出 #1

```
5
```

## 提示

$0\le a\le 10^{5000}$，$1\le b\le 10^9$。

# solve

大数除小数

```c++
#include<bits/stdc++.h>

using i64 = long long;

void solve() {
    std::string a;
    int b;
    std::cin >> a >> b;
    int n = a.size();
    std::vector<int> c(n + 5);
    for (int i = 0; i < n; i++) {
        c[i] = a[i] - '0';
    }
    std::vector<int> res( + 100);
    i64 r = 0;
    for (int i = 0; i < n; i++) {
        r = r * 10 + c[i];//被除数
        res[i] = r / b;//存商
        r %= b;//余数
    }
    int idx;
    for (int i = 0; i < n; i++) {//去除前导零
        if (res[i] != 0) {
            idx = i;
            break;
        }
    }
    for (int i = idx; i < n; i++) {
        std::cout << res[i];
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--) {
        solve();
    }
    return 0;
}
```

