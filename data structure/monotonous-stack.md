# 【模板】单调栈

## 题目背景

模板题，无背景。  

2019.12.12 更新数据，放宽时限，现在不再卡常了。

## 题目描述

给出项数为 $n$ 的整数数列 $a_{1 \dots n}$。

定义函数 $f(i)$ 代表数列中第 $i$ 个元素之后第一个大于 $a_i$ 的元素的**下标**，即 $f(i)=\min_{i<j\leq n, a_j > a_i} \{j\}$。若不存在，则 $f(i)=0$。

试求出 $f(1\dots n)$。

## 输入格式

第一行一个正整数 $n$。

第二行 $n$ 个正整数 $a_{1\dots n}$。

## 输出格式

一行 $n$ 个整数表示 $f(1), f(2), \dots, f(n)$ 的值。

## 样例 #1

### 样例输入 #1

```
5
1 4 2 3 5
```

### 样例输出 #1

```
2 5 4 5 0
```

## 提示

【数据规模与约定】

对于 $30\%$ 的数据，$n\leq 100$；

对于 $60\%$ 的数据，$n\leq 5 \times 10^3$ ；

对于 $100\%$ 的数据，$1 \le n\leq 3\times 10^6$，$1\leq a_i\leq 10^9$。

# 题解

```c++
#include<iostream>
#include<stack>

using namespace std;
const int N = 3e6 + 5;
int a[N], ans[N], n;
stack<int> s;

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1; i <= n; i++) {
        while (!s.empty() && a[s.top()] < a[i]) {
            ans[s.top()] = i;//记录答案
            s.pop();//弹出接着判断下一个
        }
        s.push(i);
    }
    for (int i = 1; i <= n; i++)
        cout << ans[i] << ' ';
}
```

# P2866 [USACO06NOV] Bad Hair Day S

## 题目描述

农夫约翰有 $N$ 头奶牛正在过乱头发节。

每一头牛都站在同一排面朝右，它们被从左到右依次编号为 $1, 2, \cdots, N$。编号为 $i$ 的牛身高为 $h_i$。第 $N$ 头牛在最前面，而第 $1$ 头牛在最后面。

对于第 $i$ 头牛**前面**的第 $j$ 头牛，如果 $h_i>h_{i+1}, h_i>h_{i+2}, \cdots, h_i>h_j$，那么认为第 $i$ 头牛可以看到第 $i+1$ 到第 $j$ 头牛。

定义 $C_i$ 为第 $i$ 头牛所能看到的牛的数量。请帮助农夫约翰求出 $C _ 1 + C _ 2 + \cdots + C _ N$。

## 输入格式

输入共 $N + 1$ 行。

第一行为一个整数 $N$，代表牛的个数。  
接下来 $N$ 行，每行一个整数 $a _ i$，分别代表第 $1, 2, \cdots, N$ 头牛的身高。

## 输出格式

输出共一行一个整数，代表 $C _ 1 + C _ 2 + \cdots + C _ N$。

## 输入输出样例 #1

### 输入 #1

```
6
10
3
7
4
12
2
```

### 输出 #1

```
5
```

## 说明/提示

### 数据规模与约定

对于 $100\%$ 的数据，保证 $1 \leq N \leq 8 \times 10 ^ 4$，$1 \leq h _ i \leq 10 ^ 9$。

# 题解

维护一个单调递减的栈，每次统计当前牛能被之前哪些牛看到

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n;
    cin >> n;
    stack<int> s;
    i64 ans = 0;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        while (!s.empty() && s.top() <= x) { // 维护一个单调递减的栈
            s.pop();
        }
        ans += s.size();
        s.push(x);
    }
    cout << ans << "\n";
}

// 10 3 7 4 12 2

// ans=1+1+2+1=5

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

# P2947 [USACO09MAR] Look Up S

## 题目描述

Farmer John's N (1 <= N <= 100,000) cows, conveniently numbered 1..N, are once again standing in a row. Cow i has height H\_i (1 <= H\_i <= 1,000,000).

Each cow is looking to her left toward those with higher index numbers. We say that cow i 'looks up' to cow j if i < j and H\_i < H\_j. For each cow i, FJ would like to know the index of the first cow in line looked up to by cow i.

Note: about 50% of the test data will have N <= 1,000. 

约翰的 $N(1\le N\le10^5)$ 头奶牛站成一排，奶牛 $i$ 的身高是 $H_i(1\le H_i\le10^6)$。现在，每只奶牛都在向右看齐。对于奶牛 $i$，如果奶牛 $j$ 满足 $i<j$ 且 $H_i<H_j$，我们可以说奶牛 $i$ 可以仰望奶牛 $j$。 求出每只奶牛离她最近的仰望对象。

Input

## 输入格式

1. \* Line 1: A single integer: N

\* Lines 2..N+1: Line i+1 contains the single integer: H\_i

第 $1$ 行输入 $N$，之后每行输入一个身高 $H_i$。

## 输出格式

\* Lines 1..N: Line i contains a single integer representing the smallest index of a cow up to which cow i looks. If no such cow exists, print 0.

共 $N$ 行，按顺序每行输出一只奶牛的最近仰望对象，如果没有仰望对象，输出 $0$。

## 输入输出样例 #1

### 输入 #1

```
6 
3 
2 
6 
1 
1 
2
```

### 输出 #1

```
3 
3 
0 
6 
6 
0
```

## 说明/提示

FJ has six cows of heights 3, 2, 6, 1, 1, and 2.


Cows 1 and 2 both look up to cow 3; cows 4 and 5 both look up to cow 6; and cows 3 and 6 do not look up to any cow.

【输入说明】$6$ 头奶牛的身高分别为 3,2,6,1,1,2。

【输出说明】奶牛 #1,#2 仰望奶牛 #3，奶牛 #4,#5 仰望奶牛 #6，奶牛 #3 和 #6 没有仰望对象。

【数据规模】

对于 $20\%$ 的数据：$1\le N\le10$；

对于 $50\%$ 的数据：$1\le N\le10^3$；

对于 $100\%$ 的数据：$1\le N\le10^5,1\le H_i\le10^6$。

# 题解

维护一个单调递减的栈，如果当前牛的身高比栈顶的牛的身高高，就记录答案

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n), ans(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    stack<int> stk;
    for (int i = 0; i < n; i++) {
        while (!stk.empty() && a[stk.top()] < a[i]) { // 维护一个单调递减的栈
            ans[stk.top()] = i + 1;
            stk.pop();
        }
        stk.push(i);
    }
    for (auto i : ans) {
        cout << i << "\n";
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

