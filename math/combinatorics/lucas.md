# 组合数(卢卡斯定理)

lucas
$$
{n \choose m}={n/p \choose m/p}*{n\pmod p \choose m\pmod p}\pmod p
$$

此方法适用于n和m非常大，但p比较小的运算

阶乘从0到1都要计算出来

# 【模板】卢卡斯定理/Lucas 定理

## 题目背景

这是一道模板题。

## 题目描述

给定整数 $n, m, p$ 的值，求出 $C_{n + m}^n \bmod p$ 的值。

输入数据保证 $p$ 为质数。

注: $C$ 表示组合数。

## 输入格式

**本题有多组数据**。

第一行一个整数 $T$，表示数据组数。

对于每组数据: 

一行，三个整数 $n, m, p$。

## 输出格式

对于每组数据，输出一行，一个整数，表示所求的值。

## 样例 #1

### 样例输入 #1

```
2
1 2 5
2 1 5
```

### 样例输出 #1

```
3
3
```

## 提示

对于 $100\%$ 的数据，$1 \leq n, m, p \leq 10^5$，$1 \leq T \leq 10$。

```c++
#include<iostream>
using namespace std;
#define ll long long
const int N=1e5+5;
ll p,n,m,t,fac[N],inv[N];
ll ksm(ll x,ll y){//快速幂
    ll res=1;
    while(y){
        if(y&1)
            res=res*x%p;
        x=x*x%p;
        y>>=1;
    }
    return res;
}
void init(ll t){//初始化
    fac[0]=inv[0]=1;
    for(int i=1;i<=t;i++){
        fac[i]=fac[i-1]*i%p;
        inv[i]=inv[i-1]*ksm(i,p-2)%p;
    }
}
ll solve(ll n,ll m){//求组合数
    if(m>n)//没有这种情况
        return 0;
    return (ll)fac[n]*inv[m]*inv[n-m]%p;
}
ll lucas(ll n,ll m){//卢卡斯定理
    if(m==0)//m肯定比n小，从任何书数中取0个的方案数为1
        return 1;
    return lucas(n/p,m/p)*solve(n%p,m%p)%p;
}
int main(){
    cin>>t;
    while(t--){
        cin>>n>>m>>p;
        init(p);//由于m和n都会被降到比p小，预处理出1到p的阶乘和阶乘的逆元
        cout<<lucas(n+m,n)<<'\n';
    }
    return 0;
}
```

