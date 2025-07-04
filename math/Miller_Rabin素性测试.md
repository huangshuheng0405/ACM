# Fermat素性测试

#### 根据费马小定理得出一种检验素数的思路：不断选取[2,n-1]中的基a，并检验是否每次都有

$$
a^{n-1}-1=1\pmod n
$$

#### 如果满足上式但n不是素数，则n被称为以a为底的伪素数，所以费马小定理的逆定理不成立，满足了上式n也不一定是素数。

# Carmichael数

#### 对于合数n，如果对于所有正整数a，a和n互素，都有费马小定理成立，则合数n为Carmichael数（卡迈克尔数）

##### 例如561=3X11X17就是卡迈克尔数，也是最小的

## 性质

### 1.卡迈克尔数无平方因子且至少有3个不同的质因子

### 2.卡迈克尔数的分布十分稀疏

# Miller-Rabin素数测试

## 二次探测定理

![](C:\Users\黄书恒1\Pictures\Code\二次探测定理.png)

```c++
ll pr[31]={2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71,73,79,83,89,97,101,103,107};
ll ksc(ll a,ll b,ll mod){//快速乘
    ll c=(long double)a/mod*b;
    ll res=(unsigned ll)a*b-(unsigned ll)c*mod;
    return (res+mod)%mod;
}
ll ksm(ll a,ll b,ll mod){//快速幂
    ll res=1;
    while(b){
        if(b&1)
            res=ksc(res,a,mod);
        a=a*a%mod;
        b>>=1;
    }
    return res;
}
bool Miller_Rabin(ll n){//素性检测
    if(n<3||n%2==0)//如果n是偶数一定不是素数
        return n==2;//特判可能是2，但2是素数
    ll u=n-1,t=0;
    while(u%2==0)//把n-1分解成u*2^t
        u/=2,++t;
    ll base[]={2,325,9375,28178,450775,9780504,1795265022};//选7个底数（缩小到两个基本可以）
    for(int i=0;i<=6;i++){
        ll a=base[i];
        ll v=ksm(a,u,n);
        if(v==1||v==n-1||v==0)//跳过本次测试
            continue;
        for(int j=1;j<=t;j++){
            v=ksc(v,v,n);//一直平方
            if(v==n-1&&j!=t){//如果在最后一次判定才出现n-1也不一定是素数
                v=1;//把v变成1后面过判断
                break;
            }
            if(v==1)//如果1出现之前没出现n-1说明n不是素数
                return 0;
        }
        if(v!=1)// 进行完t次测试后v自乘还原成了a^(2-1)，如果不等于1就不符合费马小定理就不是素数
            return 0;
    }
    return 1;
}
bool primejudge(ll x){
    for(ll i=0;i<=25&&pr[i]*pr[i]<=x;i++)//把前二十五个质数的倍数筛掉，剪枝
        if(x%pr[i]==0)
            return false;
    if(Miller_Rabin(x))//素数判定
        return true;
    return false;
}
```

![](C:\Users\黄书恒1\Pictures\Code\QQ截图20240712095655.png)