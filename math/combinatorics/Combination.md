# 组合数(快速幂)

从n个数取m个数的方案数
$$
C_n^m=\frac{n!}{m!\cdot (n-m)!}
$$
根据公式可以预处理,把所有范围内的阶乘和阶乘的逆元求出来

条件：$n,m$不是很大$1e6$，且取模的数为素数

因为取模的数为素数，所以可以用费马小定理求乘法逆元

# 组合数问题

## 题目描述

给出 $T$ 次询问，每次给出 $n,m$，请求出 $\binom{n}{m}$ 对 $998,244,353$ 取模的结果。

其中 $\binom{n}{m}$ 为二项式系数，它的另一种写法是 $C_n^m$。

## 输入格式

输入的第一行是两个整数，分别表示询问的次数 $T$ 和所给出 $n$ 的最大值 $N$。  
接下来 $T$ 行，每行两个整数，依次表示给出的 $n$ 和 $m$。

## 输出格式

为了避免输出过大，请你输出一行一个整数，表示所有询问的结果的**按位异或和**。

## 样例 #1

### 样例输入 #1

```
3 5
3 3
4 2
5 3
```

### 样例输出 #1

```
13
```

## 提示

### 样例 1 解释

三组询问的答案依次是 $1, 6, 10$。

### 数据规模与约定

对 $100\%$ 的数据，保证 $1 \leq T \leq 5 \times 10^6$，$0 \leq m \leq n \leq N \leq 5 \times 10^6$。

### 提示

请注意大量的数据读入对程序效率造成的影响，选择合适的读入方式，避免超时。

# 题解

预处理$1\sim n$的阶乘和逆元

```c++
#include <bits/stdc++.h>

using i64 = long long;

constexpr int P = 998244353;
constexpr int L = 5E6 + 5;

i64 fac[L + 1], invfac[L + 1];
i64 sumbinom[L + 1][7];

i64 binom(i64 n, i64 m) {
    if (n < m || m < 0) {
        return 0;
    }
    return 1LL * fac[n] * invfac[m] % P * invfac[n - m] % P;
}

i64 power(i64 a, i64 b) {
    i64 res = 1;
    for (; b; b /= 2, a = 1LL * a * a % P) {
        if (b % 2) {
            res = 1LL * res * a % P;
        }
    }
    return res;
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);

    fac[0] = 1;
    for (int i = 1; i <= L; i++) {
        fac[i] = 1LL * fac[i - 1] * i % P;
    }
    invfac[L] = power(fac[L], P - 2);
    for (int i = L; i; i--) {
        invfac[i - 1] = 1LL * invfac[i] * i % P;
    }

    int t, n;
    std::cin >> t >> n;

    i64 ans = 0;
    while (t--) {
        i64 x, y;
        std::cin >> x >> y;

        ans ^= binom(x, y);
    }

    std::cout << ans << "\n";
    return 0;
}
```

### 组合恒等式

$$
{n \choose m}={n-1 \choose m-1}+{n-1 \choose m}
$$

也是杨辉三角

考虑第n个元素取或不取

若取，则前n-1个元素中取m-1个

若取，则前n-1个元素中取m个

> 有时候题目数据范围有负数，那么两边的边界一定要取**-INF**

```c++
for(int i=1;i<=2000;i++)
        c[i][0]=c[i][i]=1;//杨辉三角边界等于1
    for(int i=2;i<=2000;i++)
        for(int j=1;j<i;j++)
            c[i][j]=(c[i-1][j]+c[i-1][j-1]);//杨辉三角递推式
```

# 二项式定理

$$
(a+b)^n=\sum^n_{i=0}{n \choose i} a^{n-i}b^i
$$

