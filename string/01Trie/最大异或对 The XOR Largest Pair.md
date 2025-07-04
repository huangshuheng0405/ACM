# 最大异或对 The XOR Largest Pair

## 题目描述

给定 $N$ 个整数 $A_1.A_2, \cdots, A_N$ 中选出两个进行异或计算，得到的结果最大是多少？

## 输入格式

第一行一个整数 $N$，第二行 $N$ 个整数 $A_1.A_2, \cdots, A_N$。

## 输出格式

一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
3
1 2 3
```

### 样例输出 #1

```
3
```

## 提示

对于所有测试数据，$1 \le n \le 100000$，保证 $A_i$ 在 `int` 范围内。

# 题解

先用所有的数建立一个01Trie，在枚举每个数走一遍Trie，每次走与当前位异或为1的那条路（贪心），这样高位先为1一定是最大的

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
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
int tr[N*31][2],idx;
vector<int>a(N);
void insert(int x) {//     建立字典树
    int p=0;
    for(int i=30;i>=0;i--) {
        int k=(x>>i)&1;//     从高位往低位取，否则不一定最大
        if(!tr[p][k]) {
            tr[p][k]=++idx;
        }
        p=tr[p][k];
    }
}
int query(int x) {
    int p=0,res=0;
    for(int i=30;i>=0;i--) {
        int k=(x>>i)&1;
        if(tr[p][k^1]) {//     走与当前为异或为1的那条路
            res|=(1<<i);//     答案加上当前比较的二进制位的十进制数
            p=tr[p][k^1];
        }else {
            p=tr[p][k];
        }
    }
    return res;
}
void solve() {
    int n;
    cin>>n;
    for(int i=1;i<=n;i++) {
        cin>>a[i];
        insert(a[i]);
    }
    int ans=0;
    for(int i=1;i<=n;i++) {//     枚举每个数走一遍01字典树找最大答案
        int res=query(a[i]);
        if(res>ans) {
            ans=res;
        }
    }
    cout<<ans;
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

