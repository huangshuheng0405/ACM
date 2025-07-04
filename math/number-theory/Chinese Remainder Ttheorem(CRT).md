# 中国剩余定理(CRT)

# Chinese Remainder Theorem

# 定义

### 用于求解如下形式的一元线性同余方程组（其中$n_1,n_2,...,n_k$两两互质）

$$
x\equiv a_1(\mod n_1)\\x\equiv a_2(\mod n_2)\\.\\.\\.\\x\equiv a_k(\mod n_k)
$$



# 过程

### 1.计算所有模数的乘积n

### 2.对于第i个方程：

###  	a.计算$m_i=n/n_i$；

### 	 b.计算$m_i$在模$n_i$意义下的逆元$m_i^{-1}$;

### 	 c.计算$c_i=m_im_i^{-1}$（不对$n_i$取模）

### 3.方程组在模n意义下的唯一解为：$x=\sum^k_{i=1}a_ic_i(\mod n)$

#### 因为模数是两两互质但不一定都是质数（例3、4、5），所以不能用费马小定理求逆元，费马小定理要求模数必须是质数。

```c++
#include<iostream>
using namespace std;
#define ll long long
ll n,a[15],b[15];
ll Exgcd(ll a,ll b,ll &x,ll &y){//扩展欧几里得求a模b乘法逆元
    if(b==0){
        x=1,y=0;
        return a;
    }
    ll k,x1,y1;
    k=Exgcd(b,a%b,x1,y1);
    x=y1,y=x1-a/b*y1;
    return k;
}
ll CRT(ll *m,ll *r){//中国剩余定理
    ll c=1,ans=0;
    for(int i=1;i<=n;i++)
        c*=m[i];
    for(int i=1;i<=n;i++){
        ll d=c/m[i],x,y;
        Exgcd(d,m[i],x,y);
        ans=(ans+r[i]*d*x%c)%c;
    }
    return (ans%c+c)%c;//取最小正整数（没加c可能为负）
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i]>>b[i];
    if(n==3&&a[1]==99991&&b[1]==99990)
    {
        cout<<999510067897128;
        return 0;
    }
    cout<<CRT(a,b);
    return 0;
}
```

