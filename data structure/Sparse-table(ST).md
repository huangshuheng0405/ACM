# ST表

> 用于解决**可重复贡献问题**的数据结构，避免有些重叠部分被重复计算导致超时

可以解决区间最大（小）值，也就是RMQ（RangeMaximum/Minimum Query）,找区间内的最大值,适用多次询问,询问的时间复杂度为$O(1)$

st表基于倍增思想可以做到$O(nlogn)$预处理，$O(1)$回答询问，但**不支持修改操作**

### 预处理

令$st(i,j)$表示区间$[i,i+2^j-1]$的最大值

显然$st(i,0)=a_i$

第二维就相当于倍增的时候跳了$2^{j-1}$步，于是可以得出状态转移方程：
$$
\large st(i,j)=max(st(i,j-1),st(i+2^{j-1},j-1))
$$

![img](https://oi-wiki.org/ds/images/st-preprocess-lift.svg)

### 查询

对于每个查询l和r，我们把它分成两个部分$[l,l+2^k-1]$和$[r-2^k+1,r]$，其中$k=log_2(r-l+1)$​，两部分结果的最大值就是答案

![ST 表的查询过程](https://oi-wiki.org/ds/images/st-query.svg)

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> st(n + 1, vector<int>(22));
    for (int i = 1; i <= n; i++) {
        cin >> st[i][0];
    }
    for (int j = 1; j <= 20; j++) {
        for (int i = 1; i + (1 << j) - 1 <= n; i++) {
            st[i][j] = max(st[i][j - 1], st[i + (1 << (j - 1))][j - 1]);
        }
    }
    while (m--) {
        int l, r;
        cin >> l >> r;
        int k = __lg(r - l + 1);
        cout << max(st[l][k], st[r - (1 << k) + 1][k]) << "\n";
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

# [USACO07JAN] Balanced Lineup G

## 题目描述

For the daily milking, Farmer John's N cows (1 ≤ N ≤ 50,000) always line up in the same order. One day Farmer John decides to organize a game of Ultimate Frisbee with some of the cows. To keep things simple, he will take a contiguous range of cows from the milking lineup to play the game. However, for all the cows to have fun they should not differ too much in height.

Farmer John has made a list of Q (1 ≤ Q ≤ 180,000) potential groups of cows and their heights (1 ≤ height ≤ 1,000,000). For each group, he wants your help to determine the difference in height between the shortest and the tallest cow in the group.

每天,农夫 John 的 $n(1\le n\le 5\times 10^4)$ 头牛总是按同一序列排队。

有一天, John 决定让一些牛们玩一场飞盘比赛。他准备找一群在队列中位置连续的牛来进行比赛。但是为了避免水平悬殊，牛的身高不应该相差太大。John 准备了 $q(1\le q\le 1.8\times10^5)$ 个可能的牛的选择和所有牛的身高 $h_i(1\le h_i\le 10^6,1\le i\le n)$。他想知道每一组里面最高和最低的牛的身高差。

## 输入格式

Line 1: Two space-separated integers, N and Q.


Lines 2..N+1: Line i+1 contains a single integer that is the height of cow i


Lines N+2..N+Q+1: Two integers A and B (1 ≤ A ≤ B ≤ N), representing the range of cows from A to B inclusive.

第一行两个数 $n,q$。

接下来 $n$ 行，每行一个数 $h_i$。

再接下来 $q$ 行，每行两个整数 $a$ 和 $b$，表示询问第 $a$ 头牛到第 $b$ 头牛里的最高和最低的牛的身高差。

## 输出格式

Lines 1..Q: Each line contains a single integer that is a response to a reply and indicates the difference in height between the tallest and shortest cow in the range.

输出共 $q$ 行，对于每一组询问，输出每一组中最高和最低的牛的身高差。

## 样例 #1

### 样例输入 #1

```
6 3
1
7
3
4
2
5
1 5
4 6
2 2
```

### 样例输出 #1

```
6
3
0
```

# 题解

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n, q;
    cin >> n >> q;
    vector<vector<int>> Max(n + 1, vector<int>(22)),
        Min(n + 1, vector<int>(22));
    for (int i = 1; i <= n; i++) {
        cin >> Max[i][0];
        Min[i][0] = Max[i][0];
    }
    for (int j = 1; j <= 20; j++) {
        for (int i = 1; i + (1 << j) - 1 <= n; i++) {
            Max[i][j] = max(Max[i][j - 1], Max[i + (1 << j - 1)][j - 1]);
            Min[i][j] = min(Min[i][j - 1], Min[i + (1 << j - 1)][j - 1]);
        }
    }
    while (q--) {
        int l, r;
        cin >> l >> r;
        int k = __lg(r - l + 1);
        cout << max(Max[l][k], Max[r - (1 << k) + 1][k]) -
                    min(Min[l][k], Min[r - (1 << k) + 1][k])
             << "\n";
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

