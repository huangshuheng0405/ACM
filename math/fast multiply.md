

# 快速乘

```c++
#define ll long long
ll ksc(ll a,ll b,ll mod){//快速乘
    ll c=(long double)a/mod*b;
    ll res=(unsigned ll)a*b-(unsigned ll)c*mod;
    return (res+mod)%mod;
}
```



```c++
ll ksc(ll a,ll b){//快速乘
    ll res=0;
    while(b){
        if(b&1)
            res=(res+a)%mod;
        a=(a+a)%mod;
        b>>=1;
    }
    return res;
}
```

