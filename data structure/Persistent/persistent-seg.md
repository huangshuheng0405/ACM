# P3919 【模板】可持久化线段树 1（可持久化数组）

## 题目背景

**UPDATE : 最后一个点时间空间已经放大**

2021.9.18 增添一组 hack 数据 by @panyf

标题即题意

有了可持久化数组，便可以实现很多衍生的可持久化功能（例如：可持久化并查集）

## 题目描述

如题，你需要维护这样的一个长度为 $ N $ 的数组，支持如下几种操作


1. 在某个历史版本上修改某一个位置上的值

2. 访问某个历史版本上的某一位置的值


此外，每进行一次操作（**对于操作2，即为生成一个完全一样的版本，不作任何改动**），就会生成一个新的版本。版本编号即为当前操作的编号（从1开始编号，版本0表示初始状态数组）

## 输入格式

输入的第一行包含两个正整数 $ N, M $， 分别表示数组的长度和操作的个数。

第二行包含$ N $个整数，依次为初始状态下数组各位的值（依次为 $ a_i $，$ 1 \leq i \leq N $）。

接下来$ M $行每行包含3或4个整数，代表两种操作之一（$ i $为基于的历史版本号）：

1. 对于操作1，格式为$ v_i \ 1 \ {loc}_i \ {value}_i $，即为在版本$ v_i $的基础上，将 $ a_{{loc}_i} $ 修改为 $ {value}_i $。

2. 对于操作2，格式为$ v_i \ 2 \ {loc}_i $，即访问版本$ v_i $中的 $ a_{{loc}_i} $的值，注意：**生成一样版本的对象应为 $v_i$**。

## 输出格式

输出包含若干行，依次为每个操作2的结果。

## 输入输出样例 #1

### 输入 #1

```
5 10
59 46 14 87 41
0 2 1
0 1 1 14
0 1 1 57
0 1 1 88
4 2 4
0 2 5
0 2 4
4 2 1
2 2 2
1 1 5 91
```

### 输出 #1

```
59
87
41
87
88
46
```

## 说明/提示

数据规模：

对于30%的数据：$ 1 \leq N, M \leq {10}^3 $

对于50%的数据：$ 1 \leq N, M \leq {10}^4 $

对于70%的数据：$ 1 \leq N, M \leq {10}^5 $

对于100%的数据：$ 1 \leq N, M \leq {10}^6, 1 \leq {loc}_i \leq N, 0 \leq v_i < i, -{10}^9 \leq a_i, {value}_i  \leq {10}^9$

**经测试，正常常数的可持久化数组可以通过，请各位放心**

~~数据略微凶残，请注意常数不要过大~~

~~另，此题I/O量较大，如果实在TLE请注意I/O优化~~

询问生成的版本是指你访问的那个版本的复制

样例说明：

一共11个版本，编号从0-10，依次为：

\* **0** : 59 46 14 87 41

\* **1** : 59 46 14 87 41

\* **2** : 14 46 14 87 41

\* **3** : 57 46 14 87 41

\* **4** : 88 46 14 87 41

\* **5** : 88 46 14 87 41

\* **6** : 59 46 14 87 41

\* **7** : 59 46 14 87 41

\* **8** : 88 46 14 87 41

\* **9** : 14 46 14 87 41

\* **10** : 59 46 14 87 91

# 题解

只更改$O(log\ n)$个节点，形成一条链，也就是每次修改的节点数=树的高度

可持久化线段数不能使用堆式存储法，就是不能用$x\times 2,x\times 2+1$，来表示左右儿子，而是应该动态开点，来保存每个节点左右儿子的编号。

所以我们只要在记录左右儿子的基础上，保存插入每个数的时候的根节点就可以实现可持久化了

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

const int N = 1E6 + 5;

#define lc(x) tr[x].l
#define rc(x) tr[x].r

struct Node {
    int l, r, v;
} tr[N * 25];
int a[N], idx, root[N];

void build(int &x, int l, int r) {
    x = ++idx;
    if (l == r) {
        tr[x].v = a[l];
        return;
    }
    int mid = (l + r) >> 1;
    build(lc(x), l, mid);
    build(rc(x), mid + 1, r);
}

void modify(int &x, int y, int l, int r, int pos, int k) {
    x = ++idx;
    tr[x] = tr[y]; // 新开的点要跟之前版本的节点一样
    if (l == r) {
        tr[x].v = k; // 修改目标节点
        return;
    }
    int mid = (l + r) >> 1;
    if (pos <= mid) { // 双指针同步搜
        modify(lc(x), lc(y), l, mid, pos, k);
    } else {
        modify(rc(x), rc(y), mid + 1, r, pos, k);
    }
}

int query(int x, int l, int r, int pos) {
    if (l == r) {
        return tr[x].v;
    }
    int mid = (l + r) >> 1;
    if (pos <= mid) {
        return query(lc(x), l, mid, pos);
    } else {
        return query(rc(x), mid + 1, r, pos);
    }
}

void solve() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    build(root[0], 1, n); // 版本0表示初始状态数组
    for (int i = 1; i <= m; i++) {
        int ver, op, x, y;
        cin >> ver >> op;
        if (op == 1) {
            cin >> x >> y;
            modify(root[i], root[ver], 1, n, x, y);
        } else {
            cin >> x;
            cout << query(root[ver], 1, n, x) << "\n";
            root[i] = root[ver]; // 生成一个一样的版本 只需取根节点
        }
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

