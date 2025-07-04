# 滑动窗口 /【模板】单调队列

## 题目描述

有一个长为 $n$ 的序列 $a$，以及一个大小为 $k$ 的窗口。现在这个从左边开始向右滑动，每次滑动一个单位，求出每次滑动后窗口中的最大值和最小值。

例如，对于序列 $[1,3,-1,-3,5,3,6,7]$ 以及 $k = 3$，有如下过程：

$$\def\arraystretch{1.2}
\begin{array}{|c|c|c|}\hline
\textsf{窗口位置} & \textsf{最小值} & \textsf{最大值} \\ \hline
\verb![1   3  -1] -3   5   3   6   7 ! & -1 & 3 \\ \hline
\verb! 1  [3  -1  -3]  5   3   6   7 ! & -3 & 3 \\ \hline
\verb! 1   3 [-1  -3   5]  3   6   7 ! & -3 & 5 \\ \hline
\verb! 1   3  -1 [-3   5   3]  6   7 ! & -3 & 5 \\ \hline
\verb! 1   3  -1  -3  [5   3   6]  7 ! & 3 & 6 \\ \hline
\verb! 1   3  -1  -3   5  [3   6   7]! & 3 & 7 \\ \hline
\end{array}
$$

## 输入格式

输入一共有两行，第一行有两个正整数 $n,k$。
第二行 $n$ 个整数，表示序列 $a$

## 输出格式

输出共两行，第一行为每次窗口滑动的最小值   
第二行为每次窗口滑动的最大值

## 样例 #1

### 样例输入 #1

```
8 3
1 3 -1 -3 5 3 6 7
```

### 样例输出 #1

```
-1 -3 -3 -3 3 3
3 3 5 5 6 7
```

## 提示

【数据范围】    
对于 $50\%$ 的数据，$1 \le n \le 10^5$；  
对于 $100\%$ 的数据，$1\le k \le n \le 10^6$，$a_i \in [-2^{31},2^{31})$。

# Solve

单调队列`deq`存的是数组的下标

找最小值

- 维护窗口内最前面是最小值（从前往后单调递增）
- 判断队头元素下标是否超出窗口

找最大值

- 原理同上，维护一个从前往后单调递减的单调队列

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n, k;
    std::cin >> n >> k;

    std::vector<int> a(n + 1);
    for (int i = 1; i <= n; i++) {
        std::cin >> a[i];
    }

    std::deque<int> deq;
    for (int i = 1; i <= n; i++) {//找最小值
        while (!deq.empty() && a[deq.back()] >= a[i]) {
            deq.pop_back();
        }
        deq.push_back(i);
        while (deq.front() < i - k + 1) {
            deq.pop_front();
        }
        if (i >= k) {
            std::cout << a[deq.front()] << " ";
        }
    }

    std::cout << "\n";
    deq.clear();

    for (int i = 1; i <= n; i++) {
        while (!deq.empty() && a[deq.back()] <= a[i]) {
            deq.pop_back();
        }
        deq.push_back(i);
        while (deq.front() < i - k + 1) {
            deq.pop_front();
        }
        if (i >= k) {
            std::cout << a[deq.front()] << " ";
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
