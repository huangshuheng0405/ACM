# 背包问题求方案数

有 *N* 件物品和一个容量是 *V* 的背包。每件物品只能使用一次。

第 $i$ 件物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

输出 **最优选法的方案数**。注意答案可能很大，请输出答案模 $10^9+7$ 的结果。

#### 输入格式

第一行两个整数，$N$，$V$用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$行，每行两个整数 $v_i,w_i$，用空格隔开，分别表示第 $i$件物品的体积和价值。

#### 输出格式

输出一个整数，表示 **方案数** 模 $10^9+7$的结果。

#### 数据范围

$0<N,V\leq 1000$

$0<v_i,w_i\leq 1000$

# 题解

$f[i]$表示背包容量为$i$时最优选法的方案数

记得初始化$f$数组，背包里不装物品也是一种方案

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int vol[1005], val[1005], dp[1005], f[1005];
constexpr int mod = 1E9 + 7;

void solve() {
    int n, v;
    cin >> n >> v;

    for (int i = 1; i <= n; i++) {
        cin >> vol[i] >> val[i];
    }

    for (int i = 0; i <= v; i++) {
        f[i] = 1;
    }

    for (int i = 1; i <= n; i++) {
        for (int j = v; j >= vol[i]; j--) {
            if (dp[j - vol[i]] + val[i] > dp[j]) {
                dp[j] = dp[j - vol[i]] + val[i];
                f[j] = f[j - vol[i]];//只是多装一件物品 并没有改变方案数
            } else if (dp[j - vol[i]] + val[i] == dp[j]) {//同价值的不同选择才累加方案数
                f[j] = (f[j] + f[j - vol[i]]) % mod;
            }
        }
    }

    cout << f[v] << "\n";
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

