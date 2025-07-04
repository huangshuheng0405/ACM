# 算数基本定理（唯一分解定理） 

给定一个数$n$，输出它的算数基本定理的形式输出

```c++
#include<iostream>
using namespace std;
int main(){
    cin>>n;
    vector<int>p,q
    for(int i=2,x=sqrt(n);i<=x;i++){
        while(n%i==0){
            p.push_back(i);//存底数
            q[p.size()]++;;//存指数
            n/=i; 
        }
    }
    if(n>1)
        p[++cnt]=n,e[cnt]=1;
    for(int i=1;i<=cnt;i++)
        cout<<p[i]<<' '<<e[i]<<endl;
}
```

# 阶乘分解

给定正整数$N$，试把阶乘$N!$分解质因数，按照算数基本定理的形式输出分解结果$p_i$和$c_i$即可

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<string>
#include<cmath>
#include<set>
#include<map>
#include<unordered_set>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e6 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
//int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左
vector<bool> vis(N);
vector<int> prime(N);
int cnt;

void getprime(int x) {
    for (int i = 2; i <= x; i++) {
        if (vis[i] == false) {
            prime[++cnt] = i;
        }
        for (int j = 1; i * prime[j] <= x; j++) {
            vis[i * prime[j]] = true;
            if (i % prime[j] == 0) {
                break;
            }
        }
    }
}

void solve() {
    int n;
    cin >> n;
    getprime(n);
    for (int i = 1; i <= cnt; i++) {
        int p = prime[i];
        int s = 0, t = n;
        while (t) {
            s += t / p; //指数
            t /= p; //接着算指数
        }
        cout << p << ' ' << s << endl;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    //cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}

```

