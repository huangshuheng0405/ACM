# BFS迷宫(输出最短的路径)

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<string>
#include<cmath>
#include<set>
#include<map>
#include<unordered_set>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e3 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1}; //  上右下左
int n;
char map1[N][N];

struct node {
    int x, y;
};

node dis[N][N];

void bfs() {
    queue<node> q;
    q.push({1, 1});
    map1[1][1] = '1';
    while (!q.empty()) {
        auto t = q.front();
        q.pop();
        if (t.x == n && t.y == n) {
            break;
        }
        for (int i = 0; i < 4; i++) {
            int tx = t.x + d[i][0];
            int ty = t.y + d[i][1];
            if (tx < 1 || tx > n || ty < 1 || ty > n || map1[tx][ty] == '1') {
                continue;
            }
            dis[tx][ty] = t;
            q.push({tx, ty});
            map1[tx][ty] = '1';
        }
    }
}

void print(int x, int y) {
    if (x == 0 && y == 0) {
        return;
    }
    auto p = dis[x][y];
    print(p.x, p.y);
    cout << x - 1 << ' ' << y - 1 << endl;
}

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> map1[i][j];
        }
    }
    bfs();
    print(n, n);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    //cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}

```

