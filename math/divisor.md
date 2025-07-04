# 约数个数定理

对于一个大于1的正整数$n$可以分解质因数
$$
\large n=\prod^k_{i=1}p_i^{a_i}=p_1^{a_1} \times p_2^{a_2}\dots \times p_k^{a_k}
$$
则$n$的正约数的个数就是
$$
f(n)=\prod^k_{i=1}(a_i+1)=(a_1+1)(a_2+1)\dots(a_k+1)
$$
其中$a_1,a_2,\dots,a_k$是$p_1,p_2,\dots,p_k$的指数


```c++
#include<iostream>
#include<map>
using namespace std;
const int mod=1e9+7;
int n,ans=1,x;
map<int,int>m;
int main(){
    cin>>n;
    while(n--){
        cin>>x;
        for(int i=2;i<=x/i;i++)
            while(x%i==0)
                x/=i,m[i]++;//把质因数存起来，m[i]存的是i的指数
        if(x>1)//x可能是一个比较大的质因数，把剩下这个质因数存起来
            m[x]++;
    }
    for(auto i:m)
        ans*=(1+i.second)%mod;//约数个数定理
    cout<<ans;
}
```

给定一个整数$N!$，求$N!$的正因子的个数，模$10^9+7$

```c++
#include <iostream>
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include<math.h>
#include<stdlib.h>
#include<algorithm>
#include<stack>
#include<queue>
#include<deque>
#include<iomanip>
#include <string.h>
#include<limits.h>
#define ll long long
#define mod 1000000007;
using namespace std;
const int N = 1e8 + 10;
int n, m, vis[N], prime[N];
//2到n的每个数的因子数求出来相乘
ll solve(int n) {
    int res = 1;
    for (int i = 2; i <= n; i++) {
        int l = i;
        for (int j = 2; j <= l; j++) {
            while (l % j == 0) {
                vis[j]++; //每个数个质因子存起来
                l /= j;
            }
        }
    }
    ll sum = 1; //1的因子个数就是1从1开始
    for (int i = 2; i <= n; i++) {
        if (vis[i] != 0) {
            sum *= (1 + vis[i]); //分解公式
            sum = sum % mod;
        }
    }
    return sum;
}

int main() {
    cin >> n;
    cout << solve(n) << endl;
    return 0;
} //

```

# 约数和定理

由约数个数定理可得正约数$d(n)$的个数

那么$n$的$d(n)$个正约数的和为
$$
\sigma(n)=(p_1^0+p_1^1+p_1^2+\dots+p_1^{a_1})(p_2^0+p_2^1+p_2^2+\dots+p_2^{a_2})\dots(p_k^0+p_k^1+p_k^2+\dots+p_k^{a_k})
$$
找m的约数（如果有约数为它的平方根，那么再往后找数相乘一定大于m，没必要往后找）

# 试除法求约数

`if(n / i != i)`的判断是为了防止n是i的平方的情况

```c++
for(i64 i = 2; 1LL * i * i <= n / i; i++) {
    if(n % i == 0) {
        ans.push_back(i);//约数
		if(n / i!= i) {//不能压入重复的数
            ans.push_back(n / i);//如果当前是约数，那么另一个也是约数
        }
    }
}
```

