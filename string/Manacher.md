# manacher(马拉车)

# 【模板】manacher

## 题目描述

给出一个只由小写英文字符 $\texttt a,\texttt b,\texttt c,\ldots\texttt y,\texttt z$ 组成的字符串 $S$ ,求 $S$ 中最长回文串的长度 。

字符串长度为 $n$。

## 输入格式

一行小写英文字符 $\texttt a,\texttt b,\texttt c,\cdots,\texttt y,\texttt z$ 组成的字符串 $S$。

## 输出格式

一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
aaa
```

### 样例输出 #1

```
3
```

## 提示

$1\le n\le 1.1\times 10^7$。

### 在$O(n)$的时间求出一个字符串的最长回文串

# 过程

##### 1.如果$i\leq r$（在盒内），$i$的对称点为$l+r-i$（盒子左边界（$l$）加上偏移量（$r-i$））。

##### （1）若$d[l+r-i]<r-i+1$（没超过右边界直接等于左半边的回文半径值），则$d[i]=d[l+r-i]$。

##### （2）若$d[l+r-i]\geq r-i+1$（对应的回文半径超过了右边界），则令$d[i]=r-i+1$，从$r+1$往后暴力枚举。

##### 2.如果$i>r$（在盒外），则从i开始暴力枚举。

##### 3.求出$d[i]$后，如果$i+d[i]-1>r$，则更新盒子左右边界$l=i-d[i]+1,r=i+d[i]-1;$

#### 回文半径有一半是`#`所以$d[i]$就是回文串的长度，但要减去中间那个

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
const int N = 22000000+5;
const int mod = 22000000 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
string a;
vector<char>s(N);
vector<int>d(N);
int idx;
void Manacher(vector<char> &s,int length) {
    d[1]=1;
    int l=0,r=1;
    for(int i=2;i<=length;i++) {
        if(i<=r) {
            d[i]=min(d[l+r-i],r-i+1);
        }
        while(s[i-d[i]]==s[i+d[i]]) {
            d[i]++;
        }
        if(i+d[i]-1>r) {
            l=i-d[i]+1;
            r=i+d[i]-1;
        }
    }
}
void solve() {
    cin>>a;
    a=' '+a;//    下标从1开始
    s[0]='$';//     边界结束的条件
    s[++idx]='#';
    int length=a.size()-1;
    for(int i=1;i<=length;i++) {//    把字符串变成奇回文串
        s[++idx]=a[i];
        s[++idx]='#';
    }
    length=idx;//    新字符串的长度
    Manacher(s,length);
    int ans=0;
    for(int i=1;i<=length;i++) {
        ans=max(ans,d[i]);
    }
    cout<<ans-1;//     减去中心那一个
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

