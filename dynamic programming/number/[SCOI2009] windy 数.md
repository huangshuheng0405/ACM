# [SCOI2009] windy 数

## 题目背景

windy 定义了一种 windy 数。

## 题目描述

不含前导零且相邻两个数字之差至少为 $2$ 的正整数被称为 windy 数。windy 想知道，在 $a$ 和 $b$ 之间，包括 $a$ 和 $b$ ，总共有多少个 windy 数？

## 输入格式

输入只有一行两个整数，分别表示 $a$ 和 $b$。

## 输出格式

输出一行一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
1 10
```

### 样例输出 #1

```
9
```

## 样例 #2

### 样例输入 #2

```
25 50
```

### 样例输出 #2

```
20
```

## 提示

#### 数据规模与约定

对于全部的测试点，保证 $1 \leq a \leq b \leq 2 \times 10^9$。

# 题解

##### 先求cnt-1位的windy数，再求cnt位的windy加起来就是答案

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
const int N = 12;
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//上右下左
int dp[N][N],a[N];// dp[i][j]表示有i位的数最高位是j的windy数有多少个

void init() {// 预处理找出所有的windy数
    for(int i=0;i<=9;i++) {
        dp[1][i]=1;
    }
    for(int i=2;i<=11;i++) {// 枚举位数
        for(int j=0;j<=9;j++) {// 枚举第i位
            for(int k=0;k<=9;k++) {// 枚举第i-1位
                if(abs(k-j)>=2) {// 相差2才计入答案
                    dp[i][j]+=dp[i-1][k];
                }
            }
        }
    }
}
int find(int x) {// 计算0到x中windy数的个数
    if(x==0) {
        return 0;
    }
    int cnt=0;
    while(x) {
        a[++cnt]=x%10;
        x/=10;
    }
    int res=0,last=-2;// last表示上一位数
    for(int i=cnt;i>=1;i--) {
        int now=a[i];
        for(int j=(i==cnt);j<now;j++) {// 如果是最高位从1枚举 否则从0枚举
            if(abs(j-last)>=2) {// 差2计入答案
                res+=dp[i][j];
            }
        }
        if(abs(now-last)<2) {// 不满足直接退出
            break;
        }
        last=now;
        if(i==1) {// 如果最后一位也满足条件 那么本身也算上
            res++;
        }
    }
    for(int i=1;i<cnt;i++) {// 小于cnt位的也要计入答案
        for(int j=1;j<=9;j++) {// 最高位从1开始枚举
            res+=dp[i][j];
        }
    }
    return res;
}
void solve() {
    init();
    int l,r;
    cin>>l>>r;
    cout<<find(r)-find(l-1);
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

