# 双指针

# [ABC098D] Xor Sum 2

## 题面翻译

给你一串数 $a$

求出满足$a_l+\cdots +a_r=a_l\operatorname{xor}\cdots\operatorname{xor}a_r,l\le r$ 的 $(i,j)$ 的数量

$1\le n\le 200000,\forall 1\le i\le n,0\le a_i<2^{20}(1048576)$

## 题目描述

[problemUrl]: https://atcoder.jp/contests/abc098/tasks/arc098_b

長さ $ N $ の整数列 $ A $ があります。

次の条件を満たす整数 $ l $, $ r $ ( $ 1\ \leq\ l\ \leq\ r\ \leq\ N $ ) の組の個数を求めてください。

- $ A_l\ xor\ A_{l+1}\ xor\ ...\ xor\ A_r\ =\ A_l\ +\ A_{l+1}\ +\ ...\ +\ A_r $

xorの説明

整数 $ c_1,\ c_2,\ ...,\ c_m $ の $ xor $ は以下のように定義されます。

- $ xor $ の値を $ X $ とおく。$ X $ を $ 2 $ 進数表記したときの $ 2^k $ ( $ 0\ \leq\ k $, $ k $ は整数 ) の位の値は、$ c_1,\ c_2,\ ...c_m $ のうち、$ 2 $ 進数表記したときの $ 2^k $ の位の値が $ 1 $ となるものが奇数個ならば $ 1 $、偶数個ならば $ 0 $ となる。

例えば、$ 3 $ と $ 5 $ の $ xor $ の値は、$ 3 $ の $ 2 $ 進数表記が $ 011 $、$ 5 $ の $ 2 $ 進数表記が $ 101 $ のため、$ 2 $ 進数表記が $ 110 $ の $ 6 $ となります。

## 输入格式

入力は以下の形式で標準入力から与えられる。

> $ N $ $ A_1 $ $ A_2 $ $ ... $ $ A_N $

## 输出格式

条件を満たす整数 $ l $, $ r $ ( $ 1\ \leq\ l\ \leq\ r\ \leq\ N $ ) の組の個数を出力せよ。

## 样例 #1

### 样例输入 #1

```
4
2 5 4 6
```

### 样例输出 #1

```
5
```

## 样例 #2

### 样例输入 #2

```
9
0 0 0 0 0 0 0 0 0
```

### 样例输出 #2

```
45
```

## 样例 #3

### 样例输入 #3

```
19
885 8 1 128 83 32 256 206 639 16 4 128 689 32 8 64 885 969 1
```

### 样例输出 #3

```
37
```

## 提示

### 制約

- $ 1\ \leq\ N\ \leq\ 2\ \times\ 10^5 $
- $ 0\ \leq\ A_i $
- 入力はすべて整数である

### Sample Explanation 1

明らかに、$ (l,r)=(1,1),(2,2),(3,3),(4,4) $ は条件を満たします。 また、$ (l,r)=(1,2) $ の場合、$ A_1\ xor\ A_2\ =\ A_1\ +\ A_2\ =\ 7 $ となるので、これも条件を満たします。 ほかに条件を満たす組はないので、答えは $ 5 $ になります。

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<cmath>
#include<set>
#include<map>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 2e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 0x3f3f3f3f;
ll a[N];

void solve() {
    int n;
    cin >> n;
    ll s1 = 0, s2 = 0, ans = 0;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1, j = 0; i <= n;) {
        while (j + 1 <= n && s1 + a[j + 1] == (s2 ^ a[j + 1])) {//j指针一直在合法区间的后一位
            j++;//合法 j指针就后移 知道不合法为止
            s1 += a[j];
            s2 ^= a[j];
        }
        ans += j - i + 1;//i指针后移 接着找合法的
        s1 -= a[i];
        s2 ^= a[i];
        i++;
    }
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# 单词背诵

## 题目描述

灵梦有 $n$ 个单词想要背，但她想通过一篇文章中的一段来记住这些单词。

文章由 $m$ 个单词构成，她想在文章中找出连续的一段，其中包含最多的她想要背的单词（重复的只算一个）。并且在背诵的单词量尽量多的情况下，还要使选出的文章段落尽量短，这样她就可以用尽量短的时间学习尽可能多的单词了。

## 输入格式

第 $1$ 行一个数 $n$，接下来 $n$ 行每行是一个长度不超过 $10$ 的字符串，表示一个要背的单词。

接着是一个数 $m$，然后是 $m$ 行长度不超过 $10$ 的字符串，每个表示文章中的一个单词。

## 输出格式

输出文件共 $2$ 行。第 $1$ 行为文章中最多包含的要背的单词数，第 $2$ 行表示在文章中包含最多要背单词的最短的连续段的长度。

## 样例 #1

### 样例输入 #1

```
3
hot
dog
milk
5
hot
dog
dog
milk
hot
```

### 样例输出 #1

```
3
3
```

## 提示

### 数据规模与约定

- 对于 $30\%$ 的数据，$n \le 50$，$m \le 500$；

- 对于 $60\%$ 的数据，$n \le 300$，$m \le 5000$；

- 对于 $100\%$ 的数据，$n \le 1000$，$m \le 10^5$。

  ```c++
  #include<iostream>
  #include<vector>
  #include<algorithm>
  #include<queue>
  #include<cstring>
  #include<cmath>
  #include<set>
  #include<map>
  
  using namespace std;
  #define ll long long
  #define ull unsigned long long
  #define endl '\n'
  const int N = 1e5 + 5;
  const int mod = 1e9 + 7;
  const ll inf = 0x3f3f3f3f3f3f;
  
  int sum, length;
  map<string, bool> word;
  map<string, int> cnt;
  string s[N];
  
  void solve() {
      int n;
      cin >> n;
      for (int i = 1; i <= n; i++) {
          string a;
          cin >> a;
          word[a] = true;//记录要背的单词
      }
      int m;
      cin >> m;
      for (int i = 1, j = 1; i <= m; i++) {
          cin >> s[i];
          if (word[s[i]] == true)
              cnt[s[i]]++;//记录出现的次数
          if (cnt[s[i]] == 1)
              sum++, length = i - j + 1;
          while (j <= i) {
              if (cnt[s[j]] == 1)//目标单词只出现一次就不变 保证目标单词出现最多 数量优先级高于长度
                  break;
              if (cnt[s[j]] >= 2)//如果后面遇到一样的就舍去前面出现的 去掉重复出现的单词 缩短长度
                  cnt[s[j]]--, j++;
              if (word[s[j]] == false)//去点非目标单词
                  j++;
          }
          length = min(length, i - j + 1);//更新最短长度
      }
      cout << sum << endl << length;
  }
  
  int main() {
      ios::sync_with_stdio(false);
      cin.tie(nullptr);
      cout.tie(nullptr);
      int T = 1;
      //cin >> T;
      while (T--)
          solve();
      return 0;
  }
  ```

  

# 逛画展

## 题目描述

博览馆正在展出由世上最佳的 $m$ 位画家所画的图画。

游客在购买门票时必须说明两个数字，$a$ 和 $b$，代表他要看展览中的第 $a$ 幅至第 $b$ 幅画（包含 $a,b$）之间的所有图画，而门票的价钱就是一张图画一元。

Sept 希望入场后可以看到所有名师的图画。当然，他想最小化购买门票的价格。

请求出他购买门票时应选择的 $a,b$，数据保证一定有解。

若存在多组解，**输出 $a$ 最小的那组**。

## 输入格式

第一行两个整数 $n,m$，分别表示博览馆内的图画总数及这些图画是由多少位名师的画所绘画的。

第二行包含 $n$ 个整数 $a_i$，代表画第 $i$ 幅画的名师的编号。

## 输出格式

一行两个整数 $a,b$。

## 样例 #1

### 样例输入 #1

```
12 5
2 5 3 1 3 2 4 1 1 5 4 3
```

### 样例输出 #1

```
2 7
```

## 提示

#### 数据规模与约定

- 对于 $30\%$ 的数据，有 $n\le200$，$m\le20$。
- 对于 $60\%$ 的数据，有 $n\le10^5$，$m\le10^3$。
- 对于 $100\%$ 的数据，有 $1\leq n\le10^6$，$1 \leq a_i \leq m\le2\times10^3$。

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<cmath>
#include<set>
#include<map>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e6 + 5;
const int mod = 1e9 + 7;
const ll inf = 0x3f3f3f3f3f3f;
int cnt[N], a[N];

void solve() {
    int length = 1e9, ans = 1, n, m, l, r;
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    cnt[a[1]] = 1;//从第二个开始找的 第一个直接加
    for (int i = 1, j = 1; j <= n;) {//i<j
        if (ans < m) {//画家数量不够 往后找
            j++;
            cnt[a[j]]++;
            if (cnt[a[j]] == 1)//第一次出现答案加1
                ans++;
        }
        if (ans == m){
            if (length > j - i + 1)//更新最短长度
                length = j - i + 1, l = i, r = j;
            cnt[a[i]]--;//向右接着找有没有更小的长度
            if (cnt[a[i]] == 0)//没了答案就减1
                ans--;
            i++;
        }
    }
    cout << l << ' ' << r;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# 连续自然数和

## 题目描述

对一个给定的正整数 $M$，求出所有的连续的正整数段（每一段至少有两个数），这些连续的自然数段中的全部数之和为 $M$。

例子：$1998+1999+2000+2001+2002 = 10000$，所以从 $1998$ 到 $2002$ 的一个自然数段为 $M=10000$ 的一个解。

## 输入格式

包含一个整数的单独一行给出 $M$ 的值（$10 \le M \le 2,000,000$）。

## 输出格式

每行两个正整数，给出一个满足条件的连续正整数段中的第一个数和最后一个数，两数之间用一个空格隔开，所有输出行的第一个按从小到大的升序排列，对于给定的输入数据，保证至少有一个解。

## 样例 #1

### 样例输入 #1

```
10000
```

### 样例输出 #1

```
18 142 
297 328 
388 412 
1998 2002
```

# 尺取法

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<queue>
#include<cstring>
#include<cmath>
#include<set>
#include<map>

using namespace std;
#define ll long long
#define ull unsigned long long
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 0x3f3f3f3f3f3f;

void solve() {//双指针（尺取法）
    int m;
    cin >> m;
    int i = 1, j = 1, sum = 1;
    while (i <= m / 2) {//i<=j
        if (sum < m)//
            j++, sum += j;
        if (sum >= m) {
            if (sum == m)
                cout << i << ' ' << j << endl;
            sum -= i;
            i++;
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int T = 1;
    //cin >> T;
    while (T--)
        solve();
    return 0;
}
```

