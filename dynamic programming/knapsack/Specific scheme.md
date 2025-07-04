# 背包问题求具体方案

有 $N$件物品和一个容量是 $V$ 的背包。每件物品只能使用一次。

第 $i$件物品的体积是 $v_i$，价值是$w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

输出 **字典序最小的方案**。这里的字典序是指：所选物品的编号所构成的序列。物品的编号范围是$1\dots N$ 。

#### 输入格式

第一行两个整数，$N,V$，用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$行，每行两个整数 $v_i,w_i$，用空格隔开，分别表示第 $i$件物品的体积和价值。

#### 输出格式

输出一行，包含若干个用空格隔开的整数，表示最优解中所选物品的编号序列，且该编号序列的字典序最小。

物品编号范围是 $1\dots N$。

#### 数据范围

$0<N,V\leq1000$

$0<v_i,w_i\leq1000$

#### 输入样例

```
4 5 
1 2
2 4
3 4
4 6
```

#### 输出样例

```
1 4
```

# 题解

#### 状态

  $dp[i][j]$表示 从第$i$个物品到最后一个物品装入容量为$j$的背包的最优解

#### 状态转移

$$
\large dp[i][j]=max(dp[i+1][j],dp[i+1][j-vol[i]]+val[i])
$$

$dp[i][m]$就是最大价值，从$dp[1][m]$开始搜索更新路径的方案

1. 如果$dp[i][j]=dp[i+1][j]$，**不选**第$i$个物品才可以的到最优解
2. 如果$dp[i][j]=dp[i+1][j-vol[i]]+val[i]$，**必须选**第$i$个物品才可以得到最优解
3. 如果$dp[i][j]=dp[i+1][j]=dp[i+1][j-vol[i]]+val[i]$，选不选第$i$个物品都可以得到最优解，但为了字典序最小，也必须选该物品

**总结**：如果$dp[i][j]$能由$dp[i+1][j-vol[i]]+val[i]$转移得到，则必须选该物品

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int vol[1005], val[1005], dp[1005][1005];

void solve() {
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= n; i++) {
        cin >> vol[i] >> val[i];
    }

    for (int i = n; i >= 1; i--) { // 逆序取物 让最大价值落在dp[1][m]
        for (int j = 0; j <= m; j++) {
            dp[i][j] = dp[i + 1][j];
            if (j >= vol[i]) {
                dp[i][j] = max(dp[i][j], dp[i + 1][j - vol[i]] + val[i]);
            }
        }
    }

    int j = m;
    for (int i = 1; i <= n; i++) {//找路
        if (j >= vol[i] && dp[i][j] == dp[i + 1][j - vol[i]] + val[i]) {
            cout << i << " ";
            j -= vol[i];//选了第i个物品，剩余容量要减小
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

如果是完全背包，把状态转移方程改为$dp[i][j]=max(dp[i][j],dp[i][j-vol[i]]+val[i])$即可