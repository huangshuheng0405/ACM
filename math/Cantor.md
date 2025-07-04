# 康托展开

# 【模板】康托展开

## 题目描述

求 $1\sim N$ 的一个给定全排列在所有 $1\sim N$ 全排列中的排名。结果对 $998244353$ 取模。

## 输入格式

第一行一个正整数 $N$。

第二行 $N$ 个正整数，表示 $1\sim N$ 的一种全排列。

## 输出格式

一行一个非负整数，表示答案对 $998244353$ 取模的值。

## 样例 #1

### 样例输入 #1

```
3
2 1 3
```

### 样例输出 #1

```
3
```

## 样例 #2

### 样例输入 #2

```
4
1 2 4 3
```

### 样例输出 #2

```
2
```

## 提示

对于$10\%$数据，$1\le N\le 10$。

对于$50\%$数据，$1\le N\le 5000$。

对于$100\%$数据，$1\le N\le 1000000$。

```c++
#include<iostream>
using namespace std;
const int N=1e6+5,mod=998244353;
#define ll long long
ll ans,tr[N],jc[N],a[N],n;
int lowbit(int x){//提取低位幂
    return x&-x;
}
void updata(int x,int y){//更新
    while(x<=n){
        tr[x]+=y;
        x+=lowbit(x);
    }
}
ll query(int x){//查询
    int sum=0;
    while(x){
        sum+=tr[x];
        x-=lowbit(x);
    }
    return sum;
}
int main(){
    cin>>n;
    jc[0]=1;
    for(int i=1;i<=n;i++){
        jc[i]=(jc[i-1]*i)%mod;//计算阶乘
        updata(i,1);//一开始默认是1，表示一开始都没出现过
    }
    ans=0;
    for(int i=1;i<=n;i++){
        cin>>a[i];
        ans=(ans+((query(a[i])-1)*jc[n-i])%mod)%mod;//查询sum值就是求1到a[i]-1的区间和,比a[i]小的数都在他前面
        updata(a[i],-1);//把a[i]变成0，表示出现过了，不算在sum值里面
    }
    cout<<ans+1;
    return 0;
}
/*康托展开
 * 计算当前排列在全排列里面是第几个
 * 答案为每一位数的sum(a[i])*(n-i)!的和
 *sum(a[i])在a[i]后面的元素比它小的元素个数
*/
```

