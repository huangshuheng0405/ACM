# 匈牙利算法

Hungarian Algorithm

通过不停地找增广路来增加匹配边。找不到增广路时，达到最大匹配。可以用$BFS$或$DFS$来实现

# 【模板】二分图最大匹配

## 题目描述

给定一个二分图，其左部点的个数为 $n$，右部点的个数为 $m$，边数为 $e$，求其最大匹配的边数。

左部点从 $1$ 至 $n$ 编号，右部点从 $1$ 至 $m$ 编号。

## 输入格式

输入的第一行是三个整数，分别代表 $n$，$m$ 和 $e$。

接下来 $e$ 行，每行两个整数 $u, v$，表示存在一条连接左部点 $u$ 和右部点 $v$ 的边。

## 输出格式

输出一行一个整数，代表二分图最大匹配的边数。

## 样例 #1

### 样例输入 #1

```
1 1 1
1 1
```

### 样例输出 #1

```
1
```

## 样例 #2

### 样例输入 #2

```
4 2 7
3 1
1 2
3 2
1 1
4 2
4 1
1 1
```

### 样例输出 #2

```
2
```

## 提示

#### 数据规模与约定

对于全部的测试点，保证：
- $1 \leq n, m \leq 500$。
- $1 \leq e \leq 5 \times 10^4$。
- $1 \leq u \leq n$，$1 \leq v \leq m$。

**不保证给出的图没有重边**。

# 题解

|      e[i]       |       h[u]        |    vis[v]     |   match[v]    |
| :-------------: | :---------------: | :-----------: | :-----------: |
| 存第i条边{v,ne} | 存u点的第一条出边 | v点是否访问过 | v点是否被匹配 |

1.枚举n个男生

每轮$vis$初始化为0（即女生皆可选）

深搜若能配成对，ans+1

2.枚举男生u的心仪妹子v

（1）若妹子已标记，则跳过

（2）若妹子没男友，则配成对；若妹子的男友可以让出，则配对

（3）否则，枚举u的下一个心仪妹子

3.枚举完u的心仪妹子，都不能配成对，则返回false

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n, m, e;
    std::cin >> n >> m >> e;

    std::vector<std::vector<int>> adj(501 + 1);
    std::vector<int> match(501);
    for (int i = 0; i < e; i++) {
        int u, v;
        std::cin >> u >> v;

        adj[u].push_back(v);
    }

    int ans = 0;
    for (int i = 1; i <= n; i++) {
        std::vector<bool> vis(501 + 1, false);
        auto dfs = [&](auto &&self, int u) -> bool {
            for (auto v : adj[u]) {
                if (vis[v]) {
                    continue;
                }
                vis[v] = true;
                if (!match[v] || self(self, match[v])) {
                    match[v] = u;
                    return true;
                }
            }
            return false;
        };
        if (dfs(dfs, i)) {
            ans++;
        }
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

