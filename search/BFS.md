# BFS(Breadth First Search)

### 在一个无权图上求从起点到其他点的最短路径

#### 回溯起点不能标记

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
struct node
{
    int x, y, step;
};
void solve()
{
    int n;
    cin >> n;
    vector<vector<char>> a(n + 1, vector<char>(n + 1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            cin >> a[i][j];
        }
    }
    int x1, x2, y1, y2;
    cin >> x1 >> y1 >> x2 >> y2;
    queue<node> q;
    vector<vector<bool>> vis(n + 1, vector<bool>(n + 1));
    q.push({x1, y1, 0});
    vis[x1][y1] = true;
    while (!q.empty())
    {
        auto [x, y, step] = q.front();
        q.pop();
        if (x == x2 && y == y2)
        {
            cout << step;
            return;
        }
        for (int i = 0; i <= 3; i++)
        {
            int dx = x + d[i][0];
            int dy = y + d[i][1];
            if (dx < 1 || dx > n || dy < 1 || dy > n || vis[dx][dy] == true || a[dx][dy] == '1')
            {
                continue;
            }
            vis[dx][dy] = true;
            q.push({dx, dy, step + 1});
        }
    }
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

### 二进制状态压缩

钥匙要用个变量取出来判断

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

constexpr int dx[4] = {-1, 1, 0, 0};
constexpr int dy[4] = {0, 0, -1, 1};
char a[55][55];
bool vis[55][55][17000];

void solve() {
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> a[i][j];
        }
    }

    queue<array<int, 4>> q;
    q.push({1, 1, 0, 0});
    vis[1][1][0] = true;
    while (!q.empty()) {
        auto [x, y, step, key] = q.front();
        q.pop();
        if (x == n && y == m) {
            cout << step << "\n";
            return;
        }
        for (int i = 0; i < 4; i++) {
            int tx = x + dx[i], ty = y + dy[i];
            int key1 = key; // 钥匙要单独取出来
            if (1 <= tx && tx <= n && 1 <= ty && ty <= m && a[tx][ty] != '#' &&
                !vis[tx][ty][key1]) {
                if (a[tx][ty] >= 'a' && a[tx][ty] <= 'n') { // 找到钥匙
                    int num = a[tx][ty] - 'a';
                    key1 |= (1 << num);
                }
                if (a[tx][ty] >= 'A' && a[tx][ty] <= 'N') { // 遇到门
                    int num = a[tx][ty] - 'A';
                    if (!(key1 & (1 << num))) { // 括号
                        continue;
                    }
                }
                vis[tx][ty][key] = true;
                q.push({tx, ty, step + 1, key1});
            }
        }
    }

    cout << "M*****F*****!" << "\n";
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

