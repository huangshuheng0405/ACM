# 逆序对

## 题目描述

猫猫 TOM 和小老鼠 JERRY 最近又较量上了，但是毕竟都是成年人，他们已经不喜欢再玩那种你追我赶的游戏，现在他们喜欢玩统计。

最近，TOM 老猫查阅到一个人类称之为“逆序对”的东西，这东西是这样定义的：对于给定的一段正整数序列，逆序对就是序列中 $a_i>a_j$ 且 $i<j$ 的有序对。知道这概念后，他们就比赛谁先算出给定的一段正整数序列中逆序对的数目。注意序列中可能有重复数字。

**Update:数据已加强。**

## 输入格式

第一行，一个数 $n$，表示序列中有 $n$个数。

第二行 $n$ 个数，表示给定的序列。序列中每个数字不超过 $10^9$。

## 输出格式

输出序列中逆序对的数目。

## 样例 #1

### 样例输入 #1

```
6
5 4 2 6 3 1
```

### 样例输出 #1

```
11
```

## 提示

对于 $25\%$ 的数据，$n \leq 2500$

对于 $50\%$ 的数据，$n \leq 4 \times 10^4$。

对于所有数据，$n \leq 5 \times 10^5$

请使用较快的输入输出

应该不会 $O(n^2)$ 过 50 万吧 by chen_zhe

# 归并排序

```c++
void msort(int l,int r){
    if(l>=r)
        return;
    int mid=(l+r)>>1;
    msort(l,mid);
    msort(mid+1,r);
    int i=l,j=mid+1,k=l;
    while(i<=mid&&j<=r) {
        if (e[i] <= e[j])//先放小的数
            b[k++] = e[j++];
        else
            b[k++] = e[i++],res += mid - i + 1;//求逆序对数目
        //从右边拿数左边有mid-i+1个数比b[j]大
    }
    //将剩余的数加进去
    while(i<=mid)
        b[k++]=e[i++];
    while(j<=r)
        b[k++]=e[j++];
    for(i=l;i<=r;i++)
        e[i]=b[i];//将排好序的数组b再赋值给数组e
}
```

# 树状数组

按**值**从大到小排序，若值相同，则按**位置**从大到小排序，这样相同的值不会统计为逆序对

树状数组维护

1. 初始时，树状数组的每个节点均为$0$
2. 降序排序后后，枚举每个数，设当前数值为$v$，位置为$p$
3. 先查询树状数组的$p-1$的位置，就是查找大于v的数的个数，也就是与当前数构成逆序对的个数
4. 后对树状数组的$p$位置及后序位置做加$1$的操作，及$p$位置的数对后续位置的贡献

数值相同位置按从后往前排是因为相同的值不构成逆序对（题目已说明）

```c++
#include <bits/stdc++.h>

using i64 = long long;

template <typename T> struct Fenwick {
    int n;
    std::vector<T> a;

    Fenwick(int n_ = 0) { init(n_); }

    void init(int n_) {
        n = n_;
        a.assign(n, T{});
    }

    void add(int x, const T &v) {
        for (int i = x + 1; i <= n; i += i & -i) {
            a[i - 1] = a[i - 1] + v;
        }
    }

    T sum(int x) { // 下标从1开始 x要加1
        T ans{};
        for (int i = x; i > 0; i -= i & -i) {
            ans = ans + a[i - 1];
        }
        return ans;
    }

    T rangeSum(int l, int r) {
        return sum(r) - sum(l);
    } // 下标从1开始 l、r要加1

    int select(const T &k) {
        int x = 0;
        T cur{};
        for (int i = 1 << std::__lg(n); i; i /= 2) {
            if (x + i <= n && cur + a[x + i - 1] <= k) {
                x += i;
                cur = cur + a[x - 1];
            }
        }
        return x;
    }
};

void solve() {
    int n;
    std::cin >> n;

    std::vector<std::array<int, 2>> a(n + 1);
    for (int i = 1; i <= n; i++) {
        std::cin >> a[i][0];
        a[i][1] = i;
    }

    sort(a.begin() + 1, a.end(), [&](auto x, auto y) { //从大到小排序
        if (x[0] == y[0]) {
            return x[1] > y[1];
        }
        return x[0] > y[0];
    });

    Fenwick<int> fen(n + 1);

    i64 ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += fen.sum(a[i][1]);
        fen.add(a[i][1], 1);
    }

    std::cout << ans << "\n";
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

下标从$1$开始

```c++
template <typename T> 
struct Fenwick {
    int n;
    std::vector<T> a;

    Fenwick(int n_ = 0) { init(n_); }

    void init(int n_) {
        n = n_;
        a.assign(n, T{});
    }

    void add(int x, const T &v) { // 单点修改
        for (int i = x; i <= n; i += i & -i) {
            a[i] = a[i] + v;
        }
    }

    T sum(int x) {				 // 区间查询
        T ans{};
        for (int i = x; i > 0; i -= i & -i) {
            ans = ans + a[i];
        }
        return ans;
    }

    T rangeSum(int l, int r) {
        return sum(r) - sum(l);
    } // 下标从1开始 l、r要加1

    int select(const T &k) {
        int x = 0;
        T cur{};
        for (int i = 1 << std::__lg(n); i; i /= 2) {
            if (x + i <= n && cur + a[x + i - 1] <= k) {
                x += i;
                cur = cur + a[x - 1];
            }
        }
        return x;
    }
};
```

