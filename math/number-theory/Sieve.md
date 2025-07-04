# Sieve

### 埃拉托斯特尼筛法

Eratosthenes筛法（简称埃氏筛法），时间复杂度是$O(nloglogn)$

```c++
#include<iostream>
using namespace std;
const int N=1e5;
int prime[N],cnt;
bool vis[N];
void primejudge(int t){
    for(int i=2;i<=t;i++){
        if(!vis[i]) {
            prime[++cnt] = i;//记录当前质数
            for (int j = i; j <= t; j += i)
                vis[j] = 1;//划掉当前质数的倍数
        }
    }
}
```

# 线性筛法（欧拉筛）

```c++
std::vector<int> pri;
	std::vector<bool> vis(1E6 + 1);
	for (int i = 2; i <= 1E6; i++) {
		if (vis[i] == false) {
			pri.push_back(i);
		}

		for (auto p : pri) {
			if (i * p > m) {
				break;
			}

			vis[i * p] = true;
			if (i % p == 0) {
				break;
			}
		}
	}
```

埃氏筛法会将一个合数重复多次标记，**Euler**筛法（欧拉筛），每个合数都只被标记一次，时间复杂度$O(n)$

# 【模板】线性筛素数

## 题目背景

本题已更新，从判断素数改为了查询第 $k$ 小的素数  
提示：如果你使用  `cin` 来读入，建议使用 `std::ios::sync_with_stdio(0)` 来加速。

## 题目描述

如题，给定一个范围 $n$，有 $q$ 个询问，每次输出第 $k$ 小的素数。

## 输入格式

第一行包含两个正整数 $n,q$，分别表示查询的范围和查询的个数。

接下来 $q$ 行每行一个正整数 $k$，表示查询第 $k$ 小的素数。

## 输出格式

输出 $q$ 行，每行一个正整数表示答案。

## 样例 #1

### 样例输入 #1

```
100 5
1
2
3
4
5
```

### 样例输出 #1

```
2
3
5
7
11
```

## 提示

【数据范围】  
对于 $100\%$ 的数据，$n = 10^8$，$1 \le q \le 10^6$，保证查询的素数不大于 $n$。

Data by NaCly\_Fish.

# solve

vis数组只会标记不是素数的数

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;



void solve()
{
    int n, q;
    cin >> n >> q;
    vector<bool> vis(n + 1);
    vector<int> pri(n + 1);
    int cnt = 0;
    for (int i = 2; i <= n; i++) // 欧拉筛
    {
        if (vis[i] == false)
        {
            pri[++cnt] = i;
        }
        for (int j = 1; i * pri[j] <= n; j++)
        {
            vis[i * pri[j]] = true; // 划掉合数
            if (i % pri[j] == 0)
            {
                break;
            }
        }
    }
    while (q--)
    {
        int k;
        cin >> k;
        cout << pri[k] << endl;
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

# 筛法求约数个数

用$d_i$表示$i$的约数个数，$num_i$表示$i$的最小质因子出现次数

### 约数个数定理

定理：若$\large n=\prod^m_{i=1}p_i^{c_i}$则$\large d_i=\prod^m_{i=1}(c_i+1)$

#### 证明

$p_i^{c_i}$的约数有$p_i^0,p_i^1,\dots,p_i^{c_i}$共$(c_i+1)$个，根据乘法原理，$d(n)=\prod^m_{i=1}(c_i+1)$

### 原理

1. 当$i$为质数时，$num_i\leftarrow1,d_i\leftarrow2$，同时设$\large q=\lfloor \frac{i}{p} \rfloor$,其中$q$为$i$的最小质因子
2. 当$p$为$q$的质因子时，$num_i\leftarrow num_q+1,d_i\leftarrow \frac{d_q}{num_i}\times(num_i+1)$
3. 当$p,q$互质时，$num_i\leftarrow 1,d_i\leftarrow d_q\times(num_i+1)$

#### 简单版

若$i$是质数，$num[i]=1,d[i]=2$

在线性筛中，每个合数$m$都是被**最小的质因子**筛掉的

设$p_j$是$m$中的最小质因子，则$m$通过$m=p_j\times i$筛掉的

1. 若$i$能被$p_j$整除，则$p_j$一定是$i$的最小质因子

​	$num[m]=num[i]+1$

​	$d[i]=(a[i]+1)\times \dots,d[m]=(num[m]+1)\times \dots$（m由i递推的来）

   2.若$i$不能被$p_j$整除，则$i$不包含质因子$p_j$

​	$num[m]=1,d[m]=d[i]\times(1+1)$

```c++
std::vector<int> pri;// 存质数
bool not_prime[N];// 判断质数
int d[N], num[N];// d记录约数个数 num记录最小质因子个数

void pre(int n) {
	d[1] = 1;
	for (int i = 2; i <= n; i++) {
		if (not_prime[i] == false) {// 如果i是质数
			pri.push_back(i);
			d[i] = 2;// 约数只有自己和1
			num[i] = 1;// 质因子就是本身
		}

		for (auto j : pri) {
			if (i * j > n) {
				break;
			}
			not_prime[i * j] = true;// 筛掉i的倍数
			if (i % j == 0) {// 如果i能被j整除 则j一定是i的最小质因子
				num[i * j] = num[i] + 1;// 比i多一个质因子
				d[i * j] = d[i] / num[i * j] * (num[i * j] + 1);
				break;
			}
			num[i * j] = 1;
			d[i * j] = d[i] * 2;
		}
	}
}
```

