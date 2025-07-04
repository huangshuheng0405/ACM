# 最长异或路径

## 题目描述

给定一棵 $n$ 个点的带权树，结点下标从 $1$ 开始到 $n$。寻找树中找两个结点，求最长的异或路径。

异或路径指的是指两个结点之间唯一路径上的所有边权的异或。

## 输入格式

第一行一个整数 $n$，表示点数。

接下来 $n-1$ 行，给出 $u,v,w$ ，分别表示树上的 $u$ 点和 $v$ 点有连边，边的权值是 $w$。

## 输出格式

一行，一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
4
1 2 3
2 3 4
2 4 6
```

### 样例输出 #1

```
7
```

## 提示

最长异或序列是 $1,2,3$，答案是 $7=3\oplus 4$。   

### 数据范围

$1\le n \le 100000;0 < u,v \le n;0 \le w < 2^{31}$。

# 题解

#### 谁便指定一个根（无意义），异或同一个值两次不改变原来的结果，所以两个结点之间的异或和可以转换成两个结点到根结点的异或和的异或

#### 从trie的开始，如果能和当前dis[root，u]的当前位不同的字数走，答案就更大，否则没得选

#### 原因：高位的优先级更高

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
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1}; //  上右下左
vector<pair<int, int> > adj[N];
int ch[N << 5][2], idx, ans, dis[N];

void insert(int x) {//   建立01字典树
    int p = 0;
    for (int i = 30; i >= 0; i--) {//   从高位存 因为后面查询高位优先判断
        int k = ((x >> i) & 1);//   从高位依次取到最低位
        if (!ch[p][k]) {
            ch[p][k] = ++idx;
        }
        p = ch[p][k];
    }
}

void get(int x) {//    走建立好的字典树找答案
    int res = 0, p = 0;
    for (int i = 30; i >= 0; i--) {
        int k = ((x >> i) & 1);
        if (ch[p][k ^ 1]) {//   如果和当前位异或位1就走那条路
            p = ch[p][k ^ 1];
            res |= (1 << i);//   当前位表示的十进制数
        } else {//   否则就走原路
            p = ch[p][k];
        }
    }
    ans = max(ans, res);
}

void DFS(int u, int fa) {
    insert(dis[u]);
    get(dis[u]);
    for (int i = 0; i < adj[u].size(); i++) {
        auto [v,w] = adj[u][i];
        if (v == fa) {
            continue;
        }
        dis[v] = dis[u] ^ w;
        DFS(v, u);
    }
}

void solve() {
    int n;
    cin >> n;
    for (int i = 1; i < n; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }
    DFS(1, 0);
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

