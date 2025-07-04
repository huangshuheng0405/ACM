# 字典树（Trie）

## 应用

最基础的应用——查找一个字符串是否在**字典**中出现过

也可以在记录字符串的同时记录每个前缀出现的次数，查询前缀在**字典**中出现的次数

# 【模板】字典树

## 题目描述

给定 $n$ 个模式串 $s_1, s_2, \dots, s_n$ 和 $q$ 次询问，每次询问给定一个文本串 $t_i$，请回答 $s_1 \sim s_n$ 中有多少个字符串 $s_j$ 满足 $t_i$ 是 $s_j$ 的**前缀**。

一个字符串 $t$ 是 $s$ 的前缀当且仅当从 $s$ 的末尾删去若干个（可以为 0 个）连续的字符后与 $t$ 相同。

输入的字符串大小敏感。例如，字符串 `Fusu` 和字符串 `fusu` 不同。

## 输入格式

**本题单测试点内有多组测试数据**。  

输入的第一行是一个整数，表示数据组数 $T$。

对于每组数据，格式如下：  
第一行是两个整数，分别表示模式串的个数 $n$ 和询问的个数 $q$。  
接下来 $n$ 行，每行一个字符串，表示一个模式串。  
接下来 $q$ 行，每行一个字符串，表示一次询问。

## 输出格式

按照输入的顺序依次输出各测试数据的答案。  
对于每次询问，输出一行一个整数表示答案。

## 样例 #1

### 样例输入 #1

```
3
3 3
fusufusu
fusu
anguei
fusu
anguei
kkksc
5 2
fusu
Fusu
AFakeFusu
afakefusu
fusuisnotfake
Fusu
fusu
1 1
998244353
9
```

### 样例输出 #1

```
2
1
0
1
2
1
```

## 提示

### 数据规模与约定

对于全部的测试点，保证 $1 \leq T, n, q\leq 10^5$，且输入字符串的总长度不超过 $3 \times 10^6$。输入的字符串只含大小写字母和数字，且不含空串。

### 说明
std 的 IO 使用的是关闭同步后的 cin/cout，本题不卡常。

# 题解

$nex[u,c]$：表示结点$u$的$c$字符指向的下一个结点，或者说时结点$u$代表的字符串后面添加一个字符$c$形成的字符串的结点

$cnt$：记录前缀出现次数，如果放在`for`循环外面就是记录每个单词出现次数

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int nex[3000005][120], cnt[3000005]; // nex第二维

void solve() {
    int n, m;
    cin >> n >> m;
    int tot = 0;
    auto getnum = [&](char x) { // 字母映射
        if (x >= 'A' && x <= 'Z') {
            return x - 'A';
        } else if (x >= 'a' && x <= 'z') {
            return x - 'a' + 26;
        } else {
            return x - '0' + 52;
        }
    };
    auto insert = [&](string S) { // 插入新字符串
        int p = 0;
        for (auto i : S) {
            int c = getnum(i);
            if (!nex[p][c]) { // 如果没有 就添加新节点
                nex[p][c] = ++tot;// 开点
            }
            p = nex[p][c];
            cnt[p]++;
        }
    };
    auto query = [&](string S) { // 查找字符串
        int p = 0;
        for (auto i : S) {
            int c = getnum(i);
            if (!nex[p][c]) {
                return 0;
            }
            p = nex[p][c];
        }
        return cnt[p];
    };
    for (int i = 1; i <= n; i++) {
        string s;
        cin >> s;
        insert(s);
    }
    while (m--) {
        string s;
        cin >> s;
        cout << query(s) << "\n";
    }
    for (int i = 0; i <= tot; i++) { // memset会超时
        for (int j = 0; j <= 100; j++) {
            nex[i][j] = 0;
        }
        cnt[i]=0;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

# P1481 魔族密码

## 题目背景

风之子刚走进他的考场，就……

花花：当当当当~~偶是魅力女皇——花花！！^^（华丽出场，礼炮，鲜花）

风之子：我呕……（杀死人的眼神）快说题目！否则……-\_-###

## 题目描述

花花：……咦~~好冷~~我们现在要解决的是魔族的密码问题（自我陶醉：搞不好魔族里面还会有人用密码给我和菜虫写情书咧，哦活活，当然是给我的比较多拉\*^\_^\*）。

魔族现在使用一种新型的密码系统。每一个密码都是一个给定的仅包含小写字母的英文单词表，每个单词至少包含 $1$ 个字母，至多 $75$ 个字母。如果在一个由一个词或多个词组成的表中，除了最后一个以外，每个单词都被其后的一个单词所包含，即前一个单词是后一个单词的前缀，则称词表为一个词链。例如下面单词组成了一个词链：

- $\verb!i!$；
- $\verb!int!$；
- $\verb!integer!$。

但下面的单词不组成词链：

- $\verb!integer!$；
- $\verb!intern!$。

现在你要做的就是在一个给定的单词表中取出一些词，组成最长的词链，就是包含单词数最多的词链。将它的单词数统计出来，就得到密码了。

风之子：密码就是最长词链所包括的单词数阿……

## 输入格式

这些文件的格式是，第一行为单词表中的单词数 $N$（$1 \le N \le 2000$），下面每一行有一个单词，按字典顺序排列，中间也没有重复的单词。

## 输出格式

输出共一行，一个整数，表示密码。

## 输入输出样例 #1

### 输入 #1

```
5
i
int
integer
intern
internet
```

### 输出 #1

```
4
```

# 题解

记录前缀次数，查找一个单词包含最多前缀次数

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

void solve() {
    int n;
    cin >> n;
    int tot = 0, ans = 0;
    vector<vector<int>> nex(n * 75, vector<int>(70));
    vector<int> cnt(n * 75);
    auto insert = [&](string S) {
        int p = 0, mx = 0;
        for (auto i : S) {
            int c = i - 'a' + 1;
            if (!nex[p][c]) {
                nex[p][c] = ++tot;
            }
            p = nex[p][c];
            mx += cnt[p];
        }
        ans = max(ans, mx + 1); //+1加上自己本身
        cnt[p]++;
    };
    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;
        insert(s);
    }
    cout << ans << "\n";
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

# P2922 [USACO08DEC] Secret Message G

## 题目描述

Bessie is leading the cows in an attempt to escape! To do this, the cows are sending secret binary messages to each other.

Ever the clever counterspy, Farmer John has intercepted the first $b_i$ ($1 \le b_i \le 10,000$) bits of each of $M$ ($1 \le M \le 50,000$) of these secret binary messages.

He has compiled a list of $N$ ($1 \le N \le 50,000$) partial codewords that he thinks the cows are using. Sadly, he only knows the first $c_j$ ($1 \le c_j \le 10,000$) bits of codeword $j$.

For each codeword $j$, he wants to know how many of the intercepted messages match that codeword (i.e., for codeword $j$, how many times does a message and the codeword have the same initial bits). Your job is to compute this number.

The total number of bits in the input (i.e., the sum of the $b_i$ and the $c_j$) will not exceed  $500,000$.

贝茜正在领导奶牛们逃跑．为了联络，奶牛们互相发送秘密信息．

信息是二进制的，共有 $M$（$1 \le M \le 50000$）条，反间谍能力很强的约翰已经部分拦截了这些信息，知道了第  $i$ 条二进制信息的前 $b_i$（$1 \le b_i \le 10000$）位，他同时知道，奶牛使用 $N$（$1 \le N \le 50000$）条暗号．但是，他仅仅知道第 $j$ 条暗号的前 $c_j$（$1 \le c_j \le 10000$）位。

对于每条暗号 $j$，他想知道有多少截得的信息能够和它匹配。也就是说，有多少信息和这条暗号有着相同的前缀。当然，这个前缀长度必须等于暗号和那条信息长度的较小者。

在输入文件中，位的总数（即 $\sum b_i + \sum c_i$）不会超过 $500000$。

## 输入格式

Line $1$: Two integers: $M$ and $N$.

Lines $2 \ldots M+1$: Line $i+1$ describes intercepted code $i$ with an integer $b_i$ followed by $b_i$ space-separated `0`'s and `1`'s.

Lines $M+2 \ldots M+N+1$: Line $M+j+1$ describes codeword $j$ with an integer $c_j$ followed by $c_j$ space-separated `0`'s and `1`'s.

## 输出格式

Lines $1 \ldots N$: Line $j$: The number of messages that the $j$-th codeword could match.

## 输入输出样例 #1

### 输入 #1

```
4 5 
3 0 1 0 
1 1 
3 1 0 0 
3 1 1 0 
1 0 
1 1 
2 0 1 
5 0 1 0 0 1 
2 1 1
```

### 输出 #1

```
1 
3 
1 
1 
2
```

## 说明/提示

Four messages; five codewords. 

The intercepted messages start with 010, 1, 100, and 110.

The possible codewords start with 0, 1, 01, 01001, and 11.


0 matches only 010: 1 match

1 matches 1, 100, and 110: 3 matches

01 matches only 010: 1 match

01001 matches 010: 1 match

11 matches 1 and 110: 2 matches

# 题解

典型的字典树统计题。

首先是读入。这道题全部采用2进制，也就是说只有0和1两个字符/数字，那么我们直接把它们当成字符用好了。

然后建树，也是一般的字典树建树法，只是把字母换成了0和1而已，这里不多讲。

一边建树，一边在这串数列中走过的路径中的sum+1，sum代表有sum个单词（？）经过这个节点。

当一个单词（？）插完以后，在当前节点（即该单词的最后一个数字）的end上+1，end代表有end个单词（？）在这个节点终结。

注意可能会有重复的信息，也就是说可能会有多条信息在同一个节点终结，所以这里的end是数字而不是布尔型。

然后读入待查询的信息，就像普通字典树查询一样地往下走，一边走一边把沿途的end值加起来。

循环结束，有两种情况：一是该信息全部走完，二是再往下走没有与该信息相符的节点了。

第一种情况，当前的答案要减去当前节点的end值再加上当前节点的sum值（想一想，为什么）。

第二种情况，直接输出答案。

显然比待查询的信息长的信息，如果与待查询信息有相同前缀的话，一定会经过待查询信息终结的节点。

如果待查询信息无法终结，说明没有比该信息长且前缀是该信息的信息，所以不能加上sum。

如果信息终结，那么当前节点的sum值所包含的信息一定与其有相同前缀，但sum所包含的信息有可能刚好在该节点终结，

所以要减去end值。

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int trie[500005][3], sum[500005], cnt[500005];

void solve() {
    int M, N;
    cin >> M >> N;
    int tot = 0;
    memset(trie, -1, sizeof(trie)); // 待统计的字符为01 初始化为-1或其他
    auto add = [&](vector<int> a) {
        int p = 0;
        for (auto i : a) {
            if (trie[p][i] == -1) {
                trie[p][i] = ++tot;
            }
            p = trie[p][i];
            sum[p]++; // 统计经过该节点的字符串的个数
        }
        cnt[p]++; // 统计字符串以p结尾的个数
    };
    auto query = [&](vector<int> a) {
        int p = 0;
        int res = 0; // 统计有几个字符串是a的前缀
        for (auto i : a) {
            if (trie[p][i] == -1) { // 查不下去说明没有比当前字符串长的
                return res; // 直接返回有几个字符串是a的前缀
            }
            p = trie[p][i];
            res += cnt[p]; // 加上以当前节点为结束的个数
        }
        return res - cnt[p] + sum[p];
        // 要加上比待查询字符串长的字符串 sum包含自身所以要减去cnt
    };
    for (int i = 0; i < M; i++) {
        int k;
        cin >> k;
        vector<int> a;
        for (int i = 0; i < k; i++) {
            int x;
            cin >> x;
            a.push_back(x);
        }
        add(a);
    }
    while (N--) {
        int k;
        cin >> k;
        vector<int> a;
        for (int i = 0; i < k; i++) {
            int x;
            cin >> x;
            a.push_back(x);
        }
        cout << query(a) << "\n";
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

# P10470 前缀统计

## 题目描述

给定 $N$ 个字符串 $S_1,S_2\cdots S_N$，接下来进行 $M$ 次询问，每次询问给定一个字符串 $T$，求 $S_1 \sim S_N$ 中有多少个字符串是 $T$ 的前缀。

输入字符串的总长度不超过 $10^6$，仅包含小写字母。

## 输入格式

第一行输入两个整数 $N，M$。

接下来 $N$ 行每行输入一个字符串 $S_i$。

接下来 $M$ 行每行一个字符串 $T$ 用以询问。

## 输出格式

对于每个询问，输出一个整数表示答案。

每个答案占一行。

## 输入输出样例 #1

### 输入 #1

```
3 2
ab
bc
abc
abc
efg
```

### 输出 #1

```
2
0
```

## 说明/提示

数据范围满足 $1 \le N,M \le 10^5$

# 题解

```c++
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

int trie[1000005][120], cnt[1000005];

void solve() {
    int n, m;
    cin >> n >> m;
    int tot = 0;
    auto add = [&](string s) {
        int p = 0;
        for (auto i : s) {
            int c = i - 'a';
            if (!trie[p][c]) {
                trie[p][c] = ++tot;
            }
            p = trie[p][c];
        }
        cnt[p]++;
    };
    auto query = [&](string s) {
        int p = 0;
        int ans = 0;
        for (auto i : s) {
            int c = i - 'a';
            if (!trie[p][c]) {
                return ans;
            }
            p = trie[p][c];
            ans += cnt[p];
        }
        return ans;
    };
    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;
        add(s);
    }
    while (m--) {
        string s;
        cin >> s;
        cout << query(s) << "\n";
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int _ = 1;

    // cin >> _;
    while (_--) {
        solve();
    }
    return 0;
}
```

