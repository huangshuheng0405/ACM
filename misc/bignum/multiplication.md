# 高精度乘法

# A*B Problem

## 题目背景

高精度乘法模板题。

## 题目描述

给出两个非负整数，求它们的乘积。

## 输入格式

输入共两行，每行一个非负整数。

## 输出格式

输出一个非负整数表示乘积。

## 样例 #1

### 样例输入 #1

```
1 
2
```

### 样例输出 #1

```
2
```

## 提示

每个非负整数不超过 $10^{2000}$。

# solve

高精度乘高精度

```c++
#include<bits/stdc++.h>

using i64 = long long;

int c[50005], d[50005], e[50005];

int main() {
    std::string a, b;
    std::cin >> a >> b;
    reverse(a.begin(), a.end());
    reverse(b.begin(), b.end());
    for (int i = 1; i <= a.size(); i++) {
        c[i] = a[i - 1] - '0'; 
    }
    for (int i = 1; i <= b.size(); i++) {
        d[i] = b[i - 1] - '0';
    }
    int len = a.size() + b.size(); 
    for (int i = 1; i <= a.size(); i++) {
        for (int j = 1; j <= b.size(); j++) {
            e[i + j - 1] += c[i] * d[j];
            e[i + j] += e[i + j - 1] / 10; 
            e[i + j - 1] %= 10; 
        }
    }
    while (e[len] == 0 && len > 1) {
        len--;
    }
    for (int i = len; i > 0; i--) {
        std::cout << e[i];
    }
    return 0;
}

```

