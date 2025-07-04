# 裴蜀定理

![](C:\Users\黄书恒1\Pictures\Code\屏幕截图 2024-05-24 200252.png)

![](C:\Users\黄书恒1\Pictures\Code\屏幕截图 2024-05-24 200544.png)

```c++
#include<iostream>
using namespace std;
int ans,n,a;
int gcd(int x,int y){
    return y==0?x:gcd(y,x%y);
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a;
        ans=gcd(ans,abs(a));
    }
    cout<<ans;
    return 0;
}

```

