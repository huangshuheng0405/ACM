# 最小表示法

### 定义

##### 最小表示法是用于解决字符串最小表示问题的方法

# 字符串的最小表示

### 循环同构

##### 当字符串$S$中可以选定一个位置$i$满足

$$
S[i\dots n]+S[1\dotsi-1]=T
$$

##### 则称$S$与$T$同构

```
acbd的循环同构有acbd cbda bdac dacb
```

### 最小表示

##### $S$循环同构的所有字符串中字典序最小的字符串

### 时间复杂度

##### $O(n)$

### 过程

##### 1.初始化指针$i$为$0$，$j$为$1$；初始化匹配长度$k=0$

##### 2.比较第$k$位的大小，根据比较结果跳转相应指针。若跳转后两个指针位置相同，则随意选一个加1，以保证比较的两个字符串不同

##### 3.重复上述过程，直到比较结束

##### 4.答案为$min(i,j)$

# 【模板】最小表示法

## 题目描述

小敏和小燕是一对好朋友。

他们正在玩一种神奇的游戏，叫 Minecraft。

他们现在要做一个由方块构成的长条工艺品。但是方块现在是乱的，而且由于机器的要求，他们只能做到把这个工艺品最左边的方块放到最右边。

他们想，在仅这一个操作下，最漂亮的工艺品能多漂亮。

两个工艺品美观的比较方法是，从头开始比较，如果第 $i$ 个位置上方块不一样那么谁的瑕疵度小，那么谁就更漂亮，如果一样那么继续比较第 $i+1$ 个方块。如果全都一样，那么这两个工艺品就一样漂亮。

## 输入格式

第一行一个整数 $n$，代表方块的数目。

第二行 $n$ 个整数，每个整数按从左到右的顺序输出方块瑕疵度的值，保证其小于 $30$。

## 输出格式

一行 $n$ 个整数，代表最美观工艺品从左到右瑕疵度的值。

## 样例 #1

### 样例输入 #1

```
10
10 9 8 7 6 5 4 3 2 1
```

### 样例输出 #1

```
1 10 9 8 7 6 5 4 3 2
```

## 提示

- 对于 $20\%$ 的数据，$n\le 1000$；
- 对于 $40\%$ 的数据，$n\le 10^4$；
- 对于 $100\%$ 的数据，$n\le 3\times 10^5$。

# 题解

### 对于环的问题，通常是再复制一遍数组，破环为链

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
const int N = 6e5 + 5;//     要再复制一遍数组开两倍空间
const int mod = 1e9 + 7;
const ll inf = 1e18;
int dir[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
int n;
string a;
vector<int>s(N);
int Getmin() {
    int i=1,j=2,k=0;
    while(i<=n&&j<=n) {
        for(k=0;k<n&&s[i+k]==s[j+k];k++);
        if(s[i+k]>s[j+k]) {
            i=i+k+1;
        }else {
            j=j+k+1;
        }
        if(i==j) {
            j++;
        }
    }
    return min(i,j);
}
void solve() {
    cin>>n;
    for(int i=1;i<=n;i++) {
        cin>>a;
        int num=0;
        for(int i=0;i<a.size();i++) {
            num=num*10+(a[i]-'0');
        }
        s[i]=num;
    }
    for(int i=1;i<=n;i++) {
        s[n+i]=s[i];
    }
    int idx=Getmin();
    for(int i=0;i<n;i++) {
        cout<<s[idx+i]<<' ';
    }
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

