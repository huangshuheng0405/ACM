# 最长公共子序列

### LCS（Longest Common Subsequence）

# 【模板】最长公共子序列

## 题目描述

给出 $1,2,\ldots,n$ 的两个排列 $P_1$ 和 $P_2$ ，求它们的最长公共子序列。

## 输入格式

第一行是一个数 $n$。

接下来两行，每行为 $n$ 个数，为自然数 $1,2,\ldots,n$ 的一个排列。

## 输出格式

一个数，即最长公共子序列的长度。

## 样例 #1

### 样例输入 #1

```
5 
3 2 1 4 5
1 2 3 4 5
```

### 样例输出 #1

```
3
```

## 提示

- 对于 $50\%$ 的数据， $n \le 10^3$；
- 对于 $100\%$ 的数据， $n \le 10^5$。

# 题解

先把p1数组按1到n标记，在映射到p2数中，求p2的LIS就是正确答案

```
p1
原：3 2 1 4 5
后：1 2 3 4 5 
p2
原：1 2 3 4 5
后：3 2 1 4 5
求p1跟p2的最长公共子序列就是求p2的最长上升子序列
```

# 题解

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

int a[N], b[N], c[N], len, d[N];

int find(int x) {
    int l = 1, r = len, mid;
    while (l <= r) {
        mid = (l + r) >> 1;
        if (x > d[mid]) {
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    return l;
}

void solve() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        c[a[i]] = i;// 将p1数组标记为1~n
    }
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        b[i] = c[x];// 将p1数组的标记映射到p2数组，求p2数组的LIS即可
    }
    for (int i = 1; i <= n; i++) {
        if (b[i] > d[len]) {
            d[++len] = b[i];
        } else {
            int j = find(b[i]);
            d[j] = b[i];
        }
    }
    cout << len;
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

