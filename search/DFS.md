# DFS(Depth First Search）

# 求细胞数量

## 题目描述

一矩形阵列由数字 $0$ 到 $9$ 组成，数字 $1$ 到 $9$ 代表细胞，细胞的定义为沿细胞数字上下左右若还是细胞数字则为同一细胞，求给定矩形阵列的细胞个数。

## 输入格式

第一行两个整数代表矩阵大小 $n$ 和 $m$。

接下来 $n$ 行，每行一个长度为 $m$ 的只含字符 `0` 到 `9` 的字符串，代表这个 $n \times m$ 的矩阵。

## 输出格式

一行一个整数代表细胞个数。

## 样例 #1

### 样例输入 #1

```
4 10
0234500067
1034560500
2045600671
0000000089
```

### 样例输出 #1

```
4
```

## 提示

#### 数据规模与约定

对于 $100\%$ 的数据，保证 $1 \le n,m \le 100$。

```c++
#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <stack>
#include <unordered_map>
#include <unordered_set>
#include <string>
#include <vector>
#include <functional> //lambda递归
using namespace std;
#define ll long long
#define ull unsigned long long
#define db double
#define lb long double
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
// int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左

void solve()
{
    int n, m;
    cin >> n >> m;
    vector<vector<char>> a(n + 1, vector<char>(m + 1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            cin >> a[i][j];
        }
    }
    int ans = 0;
    function<void(int, int)> dfs = [&](int x, int y)
    {
        if (x < 1 || x > n || y < 1 || y > m || a[x][y] == '0')
        {
            return;
        }
        a[x][y] = '0'; // 标记走过的
        dfs(x + 1, y);
        dfs(x - 1, y);
        dfs(x, y + 1);
        dfs(x, y - 1);
    };
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            if (a[i][j] != '0')
            {
                ans++;
                dfs(i, j);
            }
        }
    }
    cout << ans;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--)
    {
        solve();
    }
    return 0;
}
```

### 八个方向

```c++
#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <stack>
#include <unordered_map>
#include <unordered_set>
#include <string>
#include <vector>
#include <functional> //lambda递归
using namespace std;
#define ll long long
#define ull unsigned long long
#define db double
#define lb long double
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
// int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左

void solve()
{
    int n, m;
    cin >> n >> m;
    vector<vector<char>> a(n + 1, vector<char>(m + 1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            cin >> a[i][j];
        }
    }
    int ans = 0;
    function<void(int, int)> dfs = [&](int x, int y)
    {
        if (x < 1 || x > n || y < 1 || y > m || a[x][y] == '.')
        {
            return;
        }
        a[x][y] = '.'; // 标记走过的
        for (int i = -1; i <= 1; i++)
        {
            for (int j = -1; j <= 1; j++)
            {
                int dx = x + i;
                int dy = y + j;
                dfs(dx, dy);
            }
        }
    };
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            if (a[i][j] == 'W')
            {
                ans++;
                dfs(i, j);
            }
        }
    }
    cout << ans;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--)
    {
        solve();
    }
    return 0;
}
```

### 带回溯

# 迷宫

## 题目描述

给定一个 $N \times M$ 方格的迷宫，迷宫里有 $T$ 处障碍，障碍处不可通过。

在迷宫中移动有上下左右四种方式，每次只能移动一个方格。数据保证起点上没有障碍。

给定起点坐标和终点坐标，每个方格最多经过一次，问有多少种从起点坐标到终点坐标的方案。

## 输入格式

第一行为三个正整数 $N,M,T$，分别表示迷宫的长宽和障碍总数。

第二行为四个正整数 $SX,SY,FX,FY$，$SX,SY$ 代表起点坐标，$FX,FY$ 代表终点坐标。

接下来 $T$ 行，每行两个正整数，表示障碍点的坐标。

## 输出格式

输出从起点坐标到终点坐标的方案总数。

## 样例 #1

### 样例输入 #1

```
2 2 1
1 1 2 2
1 2
```

### 样例输出 #1

```
1
```

## 提示

对于 $100\%$ 的数据，$1 \le N,M \le 5$，$1 \le T \le 10$，$1 \le SX,FX \le n$，$1 \le SY,FY \le m$。

## 题解

利用回溯来求到终点的所有方案数

```c++
#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <stack>
#include <unordered_map>
#include <unordered_set>
#include <string>
#include <vector>
#include <functional> //lambda递归
using namespace std;
#define ll long long
#define ull unsigned long long
#define db double
#define lb long double
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1}; //  上右下左

void solve()
{
    int n, m, t;
    cin >> n >> m >> t;
    vector<vector<bool>> a(n + 1, vector<bool>(m + 1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            a[i][j] = true;
        }
    }
    int x1, x2, y1, y2;
    cin >> x1 >> y1 >> x2 >> y2;
    while (t--)
    {
        int x, y;
        cin >> x >> y;
        a[x][y] = false;
    }
    int ans = 0;
    vector<vector<bool>> vis(n + 1, vector<bool>(m + 1));
    function<void(int, int)> dfs = [&](int x, int y)
    {
        if (x == x2 && y == y2)
        {
            ans++;
            return;
        }
        for (int i = 0; i < 4; i++)
        {
            int dx = x + d[i][0];
            int dy = y + d[i][1];
            if (dx < 1 || dx > n || dy < 1 || dy > m || a[dx][dy] == false || vis[dx][dy] == true)
            {
                continue;
            }
            vis[x][y] = true; // 走过的路打上标记
            dfs(dx, dy);
            vis[x][y] = false; // 回溯
        }
    };
    dfs(x1, y1);
    cout << ans;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--)
    {
        solve();
    }
    return 0;
}
```

#### 问题陈述

有一个网格，横向有 $H$ 行，纵向有 $W$ 列。对于两个整数 $i$ 和 $j$ ，即 $1 \leq i \leq H$ 和 $1 \leq j \leq W$ ，位于从上往下第 $i$ 行和从左往上第 $j$ 列的正方形（我们用 $(i, j)$ 表示）上写有一个整数 $A_{i, j}$ 。

高桥目前的位置是 $(1,1)$ 。从现在起，他会重复下到当前位置右边或下面的相邻位置，直到下到 $(H, W)$ 为止。当他移动时，不允许移动到网格之外。

如果他所走过的方格（包括最初的 $(1, 1)$ 和最后的 $(H, W)$ ）上所写的整数是不同的，那么高桥就会很高兴。请找出能使他高兴的可能路径的数目。

# solve

带回溯

```c++
#include<iostream>
#include<vector>
#include<map>

using i64 = long long; 

constexpr int dx[4] = {0, 1};
constexpr int dy[4] = {1, 0};

int main () {
	int n, m;
	std::cin >> n >> m;
	std::vector<std::vector<int>> a(n + 1, std::vector<int> (m + 1));
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			std::cin >> a[i][j];
		}
	}
	std::vector<std::vector<bool>> vis(n + 1, std::vector<bool> (m + 1));
	std::map<int, bool> mp;
	i64 ans = 0;
	auto dfs = [&](auto &&self, int x, int y) -> void {
		if (x == n && y == m) {
			ans++;
			return;
		}
		for (int i = 0; i < 2; i++) {
			int tx = x + dx[i];
			int ty = y + dy[i];
			if (1 <= tx && tx <= n && 1 <= ty && ty <= m && !vis[tx][ty] && !mp[a[tx][ty]]) {
				vis[tx][ty] = true;
				mp[a[tx][ty]] = true;
				self(self, tx, ty);
				vis[tx][ty] = false;
				mp[a[tx][ty]] = false;
			}
		}
	};
	vis[1][1] = true;
	mp[a[1][1]] = true;
	dfs(dfs, 1, 1);
	std::cout << ans << "\n";
	return 0;
}
```

