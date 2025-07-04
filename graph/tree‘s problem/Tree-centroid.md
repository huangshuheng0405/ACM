# 树的重心（tree-centroid）

### 定义

### 删除重心,使剩余各个连同块结点数的最大值最小

### 性质

1.树的重心如果不唯一，则**至多有两个**，且这两个重心**相邻**

2.以树的重心为根时，所有子树的大小都不超过整棵树大小的一半2.

3.树中所有点到某个点的距离和中，到重心的距离和是最小的，如果有两个重心，那么到他们的距离和一样

4.把两棵树通过一条边相连得到一颗新的树，那么新的树的重心在相连原来两棵树的重心的路径上

5.在一棵树上添加或删除一个叶子，那么他的重心最多只会移动一条边的距离

### 求法

在DFS中计算每个子树的大小，记录[向下]的子树的最大大小，利用总点数-当前组数的大小得到[向上]的子树的大小，然后就可以依据定义找重心

```c++
void dfs(int u,int fa) {
    siz[u]=1;
    int mx=0;
    for(int i=0;i<adj[u].size();i++) {
        int v=adj[u][i];
        if(v==fa) {
            continue;
        }
        dfs(v,u);
        siz[u]+=siz[v];
        mx=max(mx,siz[v]);
    }
    mx=max(mx,n-siz[u]);
    if(mx<ans) {
        ans=mx;
        pos=u;
    }else if(mx==ans) {//      可能有两个重心，取序号小的那个
        pos=min(pos,u);
    }
}
```

# Kay and Snowflake

## 题面翻译

输入一棵树,判断每一棵子树的重心是哪一个节点.

## 题目描述

After the piece of a devilish mirror hit the Kay's eye, he is no longer interested in the beauty of the roses. Now he likes to watch snowflakes.

Once upon a time, he found a huge snowflake that has a form of the tree (connected acyclic graph) consisting of $ n $ nodes. The root of tree has index $ 1 $ . Kay is very interested in the structure of this tree.

After doing some research he formed $ q $ queries he is interested in. The $ i $ -th query asks to find a centroid of the subtree of the node $ v_{i} $ . Your goal is to answer all queries.

Subtree of a node is a part of tree consisting of this node and all it's descendants (direct or not). In other words, subtree of node $ v $ is formed by nodes $ u $ , such that node $ v $ is present on the path from $ u $ to root.

Centroid of a tree (or a subtree) is a node, such that if we erase it from the tree, the maximum size of the connected component will be at least two times smaller than the size of the initial tree (or a subtree).

## 输入格式

The first line of the input contains two integers $ n $ and $ q $ ( $ 2<=n<=300000 $ , $ 1<=q<=300000 $ ) — the size of the initial tree and the number of queries respectively.

The second line contains $ n-1 $ integer $ p_{2},p_{3},...,p_{n} $ ( $ 1<=p_{i}<=n $ ) — the indices of the parents of the nodes from $ 2 $ to $ n $ . Node $ 1 $ is a root of the tree. It's guaranteed that $ p_{i} $ define a correct tree.

Each of the following $ q $ lines contain a single integer $ v_{i} $ ( $ 1<=v_{i}<=n $ ) — the index of the node, that define the subtree, for which we want to find a centroid.

## 输出格式

For each query print the index of a centroid of the corresponding subtree. If there are many suitable nodes, print any of them. It's guaranteed, that each subtree has at least one centroid.

## 样例 #1

### 样例输入 #1

```
7 4
1 1 3 3 5 3
1
2
3
5
```

### 样例输出 #1

```
3
2
3
6
```

## 提示

![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF685B/821209b858c843715556bbbb66f001612969fa03.png)The first query asks for a centroid of the whole tree — this is node $ 3 $ . If we delete node $ 3 $ the tree will split in four components, two of size $ 1 $ and two of size $ 2 $ .

The subtree of the second node consists of this node only, so the answer is $ 2 $ .

Node $ 3 $ is centroid of its own subtree.

The centroids of the subtree of the node $ 5 $ are nodes $ 5 $ and $ 6 $ — both answers are considered correct.

```c++
#include<iostream>
#include<vector>

#define ll long long
using namespace std;
const int N = 3e5 + 5;
int n, q, x, son[N], ans[N], fa[N];
vector<int> e[N];

void DFS(int u, int f) {
    int res = 0;
    son[u] = 1;//子树大小
    ans[u] = u;//以结点u为根的子树重心时ans[u]
    for (int v: e[u]) {
        if (v == f)
            continue;
        DFS(v, u);
        son[u] += son[v];//求出各个子树的结点数
        if (son[v] > son[res])
            res = v;
    }//树的性质：以数的重心为根时，所有子树的大小都不会超过整棵树大小的一半
    if ((son[res] * 2) > son[u]) {
        int temp = ans[res];
        while ((son[u] - son[temp]) * 2 > son[u])//向上判断路径上的结点是不是重心即可
            temp = fa[temp];//判断父节点是不是重心
        ans[u] = temp;//以u为根节点的数重心时temp
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    cin >> n >> q;
    for (int i = 2; i <= n; i++) {
        cin >> x;//第i个点的父节点
        fa[i] = x;//父节点
        e[i].push_back(x);
        e[x].push_back(i);
    }
    DFS(1, 0);
    while (q--) {
        cin >> x;
        cout << ans[x] << '\n';
    }
    return 0;
}
```

