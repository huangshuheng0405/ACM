# AC自动机

### 概述

AC自动机是**以Trie的结构为基础**，**结合KMP的思想**建立的自动机，用于解决多模式匹配的任务

### fail指针

> 回跳边指的是**父节点的回跳边所指的结点的儿子结点**

`fail[u]`存的是结点$u$的回跳边的终点，回跳边所指结点一定是当前结点的**最长后缀**

### 字典树

> 转移边：指向当前结点的回跳边所指结点的儿子

`tr[u][i]`有两种理解方式。第一种是字典树上的一条边，即结点$u$的终点，第二种是结点$u$的**转移边**的终点

### 构建函数（build）

 build 函数将结点按 BFS 顺序入队，依次求 fail 指针。这里的字典树根结点为 0，我们将根结点的子结点一一入队。若将根结点入队，则在第一次 BFS 的时候，会将根结点儿子的 fail 指针标记为本身。因此我们将根结点的儿子一一入队，而不是将根结点入队 

 然后开始 BFS：每次取出队首的结点 u（$fail[u]$在之前的 BFS 过程中已求得），然后遍历字符集（这里是 0-25，对应 a-z，即 ![u](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7) 的各个子节点）： 

1.若儿子存在，则父亲帮儿子建**回跳边**，并把儿子入队

2.若儿子不存在，则父亲自建**转移边**

# AC 自动机（简单版）

## 题目描述

给定 $n$ 个模式串 $s_i$ 和一个文本串 $t$，求有多少个不同的模式串在文本串里出现过。  
两个模式串不同当且仅当他们**编号**不同。

## 输入格式

第一行是一个整数，表示模式串的个数 $n$。  
第 $2$ 到第 $(n + 1)$ 行，每行一个字符串，第 $(i + 1)$ 行的字符串表示编号为 $i$ 的模式串 $s_i$。  
最后一行是一个字符串，表示文本串 $t$。

## 输出格式

输出一行一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
3
a
aa
aa
aaa
```

### 样例输出 #1

```
3
```

## 样例 #2

### 样例输入 #2

```
4
a
ab
ac
abc
abcd
```

### 样例输出 #2

```
3
```

## 样例 #3

### 样例输入 #3

```
2
a
aa
aa
```

### 样例输出 #3

```
2
```

## 提示

### 样例 1 解释

$s_2$ 与 $s_3$ 编号（下标）不同，因此各自对答案产生了一次贡献。

### 样例 2 解释

$s_1$，$s_2$，$s_4$ 都在串 `abcd` 里出现过。

### 数据规模与约定

- 对于 $50\%$ 的数据，保证 $n = 1$。
- 对于 $100\%$ 的数据，保证 $1 \leq n \leq 10^6$，$1 \leq |t| \leq 10^6$，$1 \leq \sum\limits_{i = 1}^n |s_i| \leq 10^6$。$s_i, t$ 中仅包含小写字母。

# 题解

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

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e6 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
int tr[N][26],idx,cnt[N],fail[N];
void insert(string a) {//     建立模式串的字典树
    int p=0;
    for(int i=0;i<a.size();i++) {
        int j=a[i]-'a';
        if(!tr[p][j]) {
            tr[p][j]=++idx;
        }
        p=tr[p][j];
    }
    cnt[p]++;//     这个字符串出现的次数
}
void build() {//      BFS建立AC自动机
    queue<int>q;
    for(int i=0;i<26;i++) {
        if(tr[0][i]) {
            q.push(tr[0][i]);
        }
    }
    while(!q.empty()) {
        int u=q.front();
        q.pop();
        for(int i=0;i<26;i++) {
            int v=tr[u][i];
            if(v) {
                fail[v]=tr[fail[u]][i];//     帮儿子建回跳边
                q.push(v);
            }else {
                tr[u][i]=tr[fail[u]][i];//     自建转移边
 						//u结点回跳边的儿子	
            }
        }
    }
}
int query(string a) {
    int ans=0;
    for(int k=0,i=0;k<a.size();k++) {
        i=tr[i][a[k]-'a'];
        for(int j=i;j&&cnt[j]!=-1;j=fail[j]) {
            ans+=cnt[j];
            cnt[j]=-1;//    清空次数
        }
    }
    return ans;
}
void solve() {
    int n;
    cin>>n;
    for(int i=1;i<=n;i++) {
        string a;
        cin>>a;
        insert(a);
    }
    build();
    string a;
    cin>>a;
    cout<<query(a);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;
    //cin >> _;
    while (_--){
        solve();
    }
    return 0;
}
```

