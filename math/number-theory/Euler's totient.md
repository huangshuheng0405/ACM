# 欧拉函数

欧拉函数（Euler's totient function），即$\varphi(n)$表示的是小于等于n和n互质的个数，比如说$\varphi(1)=1$

当n是质数的时候，显然有$\varphi(n)=n-1$。

# 性质

$$
\varphi(n)=n\times\prod^s_{i=1}((p_i-1)\div p_i)
$$

```c++
int Getphi(int n){//求欧拉函数
    int res=n;
    for(int i=2;i*i<=n;i++)
        if(n%i==0){
            res=res/i*(i-1);
            while(n%i==0)
                n/=i;
        }
    if(n>1)
        res=res/n*(n-1);
    return res;
}
```

