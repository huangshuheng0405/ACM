# 拓扑排序

给定一个DAG，如果从i到j有边，则认为j依赖于i,如果有i到j有路径(i可达j),则称j间接依赖于i。

拓扑排序的目的是将所有节点排序，使排在前面的节点不能依赖于排在后面的节点

必须是有向无环图(DAG)，不能出现环，有环会出现没有入度为0的点(不知道先遍历哪个点)

### 应用

可以判断图中是否有环，以及判断图是否是一条链

# 【模板】拓扑排序 / 家谱树

## 题目描述

有个人的家族很大，辈分关系很混乱，请你帮整理一下这种关系。给出每个人的后代的信息。输出一个序列，使得每个人的后辈都比那个人后列出。

## 输入格式

第 $1$ 行一个整数 $N$（$1 \le N \le 100$），表示家族的人数。接下来 $N$ 行，第 $i$ 行描述第 $i$ 个人的后代编号 $a_{i,j}$，表示 $a_{i,j}$ 是 $i$ 的后代。每行最后是 $0$ 表示描述完毕。

## 输出格式

输出一个序列，使得每个人的后辈都比那个人后列出。如果有多种不同的序列，输出任意一种即可。

## 样例 #1

### 样例输入 #1

```
5
0
4 5 1 0
1 0
5 3 0
3 0
```

### 样例输出 #1

```
2 4 5 3 1
```

# Kahn算法

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
    int n;
    std::cin >> n;

    std::vector<std::vector<int>> adj(n + 1);
    std::vector<int> in(n + 1);
    for (int i = 1; i <= n; i++) {
        int k;
        while (1) {
            std::cin >> k;
            if (k == 0) {
                break;
            }
            adj[i].push_back(k);
            in[k]++;
        }
    }

    std::queue<int> q;
    for (int i = 1; i <= n; i++) {
        if (in[i] == 0) {
            q.push(i);
        }
    }
    std::vector<int> ans;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        ans.push_back(u);
        for (auto v : adj[u]) {
            if (--in[v] == 0) {
                q.push(v);
            }
        }
    }

    for (auto i : ans) {
        std::cout << i << " ";
    }
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

### 求字典序最大\最小的拓扑排序

将**Kahn**算法中的队列替换成最大堆\最小堆实现的优先队列即可

# 拓扑排序模板

## 题目描述

有向无环图上有n个点，m条边。求这张图字典序最小的拓扑排序的结果。字典序最小指希望排好序的结果中，比较靠前的数字尽可能小。

## 输入格式

第一行是用空格隔开的两个整数n和m，表示n个点和m条边。

接下来是m行，每行用空格隔开的两个数u和v，表示有一条从u到v的边。

## 输出格式

输出一行，拓扑排序的结果，数字之间用空格隔开

## 样例 #1

### 样例输入 #1

```
5 3
1 2
2 4
4 3
```

### 样例输出 #1

```
1 2 4 3 5
```

## 提示

$1 \leq n,m \leq 10^5$

注意：图上可能有重边

# 题解

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+5;
vector<int>adj[N];
int ru[N];
bool cmp(int x,int y) {
    return x<y;
}
int main() {
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=m;i++) {
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
        ru[v]++;
    }
    priority_queue<int,vector<int>,greater<int> >q;
    for(int i=1;i<=n;i++) {
        if(ru[i]==0) {
            q.push(i);
        }
    }
    vector<int>ans;
    while(!q.empty()) {
        int u=q.top();
        q.pop();
        ans.push_back(u);
        for(int v:adj[u]) {
            if(--ru[v]==0) {
                q.push(v);
            }
        }
    }
    for(int i:ans) {
        cout<<i<<' ';
    }
    return 0;
}
```

