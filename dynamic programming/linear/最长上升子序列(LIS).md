# 最长上升子序列

### LIS（Longest Increasing Subsequence）

## 题目描述

这是一个简单的动规板子题。

给出一个由 $n(n\le 5000)$ 个不超过 $10^6$ 的正整数组成的序列。请输出这个序列的**最长上升子序列**的长度。

最长上升子序列是指，从原序列中**按顺序**取出一些数字排在一起，这些数字是**逐渐增大**的。

## 输入格式

第一行，一个整数 $n$，表示序列长度。

第二行有 $n$ 个整数，表示这个序列。

## 输出格式

一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
6
1 2 4 1 3 4
```

### 样例输出 #1

```
4
```

## 提示

分别取出 $1$、$2$、$3$、$4$ 即可。

# 朴素算法

#### 时间复杂度$O(n^2)$

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
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1}; //上右下左

void solve() {
    int n;
    cin >> n;
    vector<int> a(n + 5);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    vector<int> dp(n + 5);
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        dp[i] = 1; //自身长度为1
        for (int j = 1; j < i; j++) {
            if (a[j] < a[i]) {
                dp[i] = max(dp[i], dp[j] + 1);
                ans = max(ans, dp[i]);
            }
        }
    }
    cout << ans;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;
    //cin >> _;
    while (_--)
        solve();
    return 0;
}

```

# 二分查找优化

#### 时间复杂度$O(nlogn)$

考虑新进来的一个元素$a[i]$

- 大则添加：如果$a[i]$大于$b[len]$，直接让$b[++len]=a[i]$。即b数组的长度增加1，且添加了一个元素
- 小则替换：如果$a[i]$小于或等于$b[len]$，就用$a[i]$替换掉b数组中第一个大于或等于$a[i]$的元素，这样会使b这个上升序列结尾元素更小，对于一个上升子序列其结尾元素越小，越有利续接其他元素，也就越可能变得更长

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <cstring>
#include <cmath>
#include <set>
#include <map>
#include <functional>

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
    int n;
    cin >> n;
    vector<int> a(n + 1), b(n + 1);
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
    }
    int len = 1;
    b[1] = a[1];
    // auto find = [&](int x)
    // {
    //     int l = 1, r = len;
    //     while (l <= r)
    //     {
    //         int mid = (l + r) >> 1;
    //         if (x > b[mid])
    //         {
    //             l = mid + 1;
    //         }
    //         else
    //         {
    //             r = mid - 1;
    //         }
    //     }
    //     return l; // 返回左边界
    // };
    for (int i = 2; i <= n; i++)
    {
        if (a[i] > b[len])
        { // 当前元素大于 直接插在后面
            b[++len] = a[i];
        }
        else
        {                                                                  // 否则替换掉b数组中第一个大于等于a[i]的元素
            *lower_bound(b.begin() + 1, b.begin() + len + 1, a[i]) = a[i]; // lower_bound返回的是b数组的地址 直接从地址改变值
        }
    }
    cout << len << endl;
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

