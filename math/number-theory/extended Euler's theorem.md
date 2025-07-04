# 扩展欧拉定理

![](C:\Users\黄书恒1\Pictures\Code\屏幕截图 2024-05-26 234256.png)

```c++
#include<iostream>
using namespace std;
int a,b,m,p;
string s;
int Getphi(int n){//求欧拉函数
    int res=n;
    for(int i=2;i*i<=n;i++){
        if(n%i==0){
            res=res/i*(i-1);
            while(n%i==0)
                n/=i;
        }
    }
    if(n>1)
        res=res/n*(n-1);
    return res;
}
int DeclinePower(int phi){//降幂
    int c=0,flag=0;
    for(int i=0;i<s.size();i++) {
        c = c * 10 + (s[i] - '0');//秦九韶算法
        if (c >= phi)
            flag = 1, c %= phi;//什么时候取模都可以
    }
    if(flag)//降过幂要再加一次欧拉函数
        c+=phi;
    return c;
}
int QuickPower(long long x,int y){//快速幂
    int ans=1;
    while(y){
        if(y&1)
            ans=ans*x%m;
        x=x*x%m;
        y>>=1;
    }
    return ans;
}
int main(){
    cin>>a>>m>>s;
    p= Getphi(m);//求m的欧拉函数
    b=DeclinePower(p);//把b降幂
    cout<<QuickPower(a,b);
    return 0;
}
```

# 秦九韶算法

![](C:\Users\黄书恒1\Pictures\Code\屏幕截图 2024-05-27 115054.png)

