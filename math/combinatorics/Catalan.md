# 卡特兰数

### Catalan

一种操作数不能超过另一种操作数，或者两种操作不能有交集，这些操作的合法方案数，通常是卡特兰数
$$
H_n=\frac{H_{n-1}(4n-2)}{n+1}
$$

$$
H_n={2n\choose n}-{2n \choose n-1}
$$

$$
T_{1}（x）=\begin{cases} \sum_{i=1}^n H_{i-1}H_{n-i} ,n\geq 2 \\ 1, n = 0,1\end{cases}
$$



```c++
#include<iostream>
using namespace std;
#define ll long long
ll h[1005],n;
int main(){
    cin>>n;
    h[0]=h[1]=1;
    for(int i=2;i<=n;i++)
        for(int k=1;k<=i;k++){
            h[i]+=h[k-1]*h[i-k];
            h[i]%=100;
        }
    cout<<h[n];
    return 0;
}
```

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
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
//int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左

void solve() {
    vector<int> h(100);
    h[1]=1;
    for (int i = 2; i <= 10; i++) {
        h[i] = h[i - 1] * (4 * i - 2) / (i + 1);
    }
    for (int i = 1; i <= 10; i++) {
        cout << h[i] << ' ';
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

