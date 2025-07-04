# 斐波那契数列

## 性质

- #### 尾数循环

  斐波那契数列的个位数：一个60步的循环；进一步，最后两位数是一个300步的循环，最后三位数是一个1500步的循环，最后四位数是一个15000步的循环，最后五位数是一个150000步的循环

- #### 公约数和整除关系

  $F_n$整除$F_m$，当且仅当$n$整除$m$，其中$n\geq3$

  $\large gcd(F_m,F_n)=F_{gcd(m,n)}$

  任意连续三个斐波那契数两两**互素**，对于每一个$n$
  $$
  \large gcd(F_n,F_{n+1})=gcd(F_n,F_{n+2})=gcd(F_{n+1},F_{n+2})=1
  $$

- 

斐波那契数列的每位数都会循环出现，对a的b次方这么大的数直接对循环周期取模，找它对应的位置就行了，

因为其第一项是0第二项是1，这就是周期出现的条件。

```c++
#include<iostream>
using namespace std;
#define ull unsigned long long
ull a,b,n,t,ans,f[1000100],cnt;
ull ksm(ull x,ull y,ull mod){
    ull res=1;
    while(y){
        if(y&1)
            res=res*x%mod;
        x=x*x%mod;
        y>>=1;
    }
    return res%mod;
}
int main(){
    ios::sync_with_stdio(false),cin.tie(nullptr),cout.tie(nullptr);
    cin>>t;
    while(t--){
        cin>>a>>b>>n;
        if(a==0||n==1){//特判
            cout<<0<<'\n';
            continue;
        }
        cnt=0;
        f[0]=0,f[1]=1,f[2]=1;
        for(int i=2;i<=n*n;i++){//在n*n个项一定会出现重复
            f[i]=(f[i-1]+f[i-2])%n;
            cnt++;//cnt就是数列对n取模的循环周期
            if(f[i]==1&&f[i-1]==0)
                break;
        }
        ans=ksm(a%cnt,b,cnt);//答案就是ans对应在周期中的哪一项中
        cout<<f[ans]<<endl;//对应过去就是答案
    }
}
//斐波那契数列是以1开始的F(i+2)%n=(F(i+1)%n+F(i)%n)%n，当F(i+1)=F(i)=1(F(i)=1&&F(i-1)=0)数列就循环出现了
```

# 斐波那契数列

## 题目描述

大家都知道，斐波那契数列是满足如下性质的一个数列：

$$F_n = \left\{\begin{aligned} 1 \space (n \le 2) \\ F_{n-1}+F_{n-2} \space (n\ge 3) \end{aligned}\right.$$


请你求出 $F_n \bmod 10^9 + 7$ 的值。

## 输入格式

一行一个正整数 $n$

## 输出格式

输出一行一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
5
```

### 样例输出 #1

```
5
```

## 样例 #2

### 样例输入 #2

```
10
```

### 样例输出 #2

```
55
```

## 提示

【数据范围】    
对于 $60\%$ 的数据，$1\le n \le 92$；   
对于 $100\%$ 的数据，$1\le n < 2^{63}$。

# 题解

直接递推会超时，用矩阵加快速幂降低时间复杂度

```

```

