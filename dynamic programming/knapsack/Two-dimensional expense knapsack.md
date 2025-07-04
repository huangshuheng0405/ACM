# 二维费用背包

不同的是选一个物品会消耗两种价值，只需要在状态中**增加一维放第二种价值即可**，再开一维存放物品编号就不合适了，容易MLE

枚举两种状态即可

# 榨取kkksc03

## 题目描述

洛谷 2 的团队功能是其他任何 OJ 和工具难以达到的。借助洛谷强大的服务器资源，任何学校都可以在洛谷上零成本的搭建 OJ 并高效率的完成训练计划。

为什么说是搭建 OJ 呢？为什么高效呢？

![](https://cdn.luogu.com.cn/upload/pic/1236.png)

因为，你可以上传私有题目，团队外别人是无法看到的。我们还能帮你们评测！

你可以创建作业，给组员布置任务，查看组员的完成情况，还可以点评任意一份代码！

你可以创建比赛！既可以是 OI 赛制还可以是 ICPC 赛制！既可以是团队内部的私有比赛，也可以公开赛，甚至可以指定谁可以参加比赛。这样，搞“x 校联赛”最合适不过了。洛谷凭借这个功能，希望能够提供公开及私有比赛的另外一个平台。

![](https://cdn.luogu.com.cn/upload/pic/1237.png)

值得说明的是，本次比赛就是采用团队私有题目+邀请比赛的机制。

洛谷的运营组决定，如果一名 OIer 向他的教练推荐洛谷，并能够成功的使用（成功使用的定义是：该团队有 $20$ 个或以上的成员，上传 $10$ 道以上的私有题目，布置过一次作业并成功举办过一次公开比赛），那么他可以浪费掉 kkksc03 的一些时间的同时消耗掉 kkksc03 的一些金钱以满足自己的一个愿望。

kkksc03 的时间和金钱是有限的，所以他很难满足所有同学的愿望。所以他想知道在自己的能力范围内，最多可以完成多少同学的愿望？

## 输入格式

第一行三个整数 $n,M,T$，表示一共有 $n$（$1 \le n \le 100$）个愿望， kkksc03 的手上还剩 $M$（$0 \le M \le 200$）元，他的暑假有 $T$（$0 \le T \le 200$）分钟时间。

第 $2$~$n+1$ 行 $m_{i}$ , $t_{i}$ 表示第 $i$ 个愿望所需要的金钱和时间。

## 输出格式

一行，一个数，表示 kkksc03 最多可以实现愿望的个数。

## 样例 #1

### 样例输入 #1

```
6 10 10
1 1
2 3 
3 2
2 5
5 2
4 3
```

### 样例输出 #1

```
4
```

# solve

01背包+两维价值

```c++
#include<bits/stdc++.h>

using i64 = long long;
using ld = long double;

int main () {
	std::ios::sync_with_stdio(false);	
	std::cin.tie(nullptr);

	int n, M, T;
	std::cin >> n >> M >> T;

	std::vector<int> m(n), t(n);
	for (int i = 0; i < n; i++) {
		std::cin >> m[i] >> t[i];//金钱 时间
	}

	std::vector<std::vector<int>> dp(M + 1, std::vector<int> (T + 1));
	for (int i = 0; i < n; i++) {// 枚举物品
		for (int j = M; j >= m[i]; j--) {// 枚举金钱
			for (int k = T; k >= t[i]; k--) {// 枚举时间
				dp[j][k] = std::max(dp[j][k], dp[j - m[i]][k - t[i]] + 1);
			}
		}
	}

	std::cout << dp[M][T] << "\n";

	return 0;
}
```

# NASA的食物计划

## 题目背景

NASA（美国航空航天局）因为航天飞机的隔热瓦等其他安全技术问题一直大伤脑筋，因此在各方压力下终止了航天飞机的历史，但是此类事情会不会在以后发生，谁也无法保证。所以，在遇到这类航天问题时，也许只能让航天员出仓维修。但是过多的维修会消耗航天员大量的能量，因此 NASA 便想设计一种食品方案，使体积和承重有限的条件下多装载一些高卡路里的食物。

## 题目描述

航天飞机的体积有限，当然如果载过重的物品，燃料会浪费很多钱，每件食品都有各自的体积、质量以及所含卡路里。在告诉你体积和质量的最大值的情况下，请输出能达到的食品方案所含卡路里的最大值，当然每个食品只能使用一次。

## 输入格式

第一行 $2$ 个整数，分别代表体积最大值 $h$ 和质量最大值 $t$。 

第二行 $1$ 个整数代表食品总数 $n$。 

接下来 $n$ 行每行 $3$ 个数 体积 $h_i$，质量 $t_i$，所含卡路里 $k_i$。

## 输出格式

一个数，表示所能达到的最大卡路里（`int` 范围内）

## 样例 #1

### 样例输入 #1

```
320 350
4
160 40 120
80 110 240
220 70 310
40 400 220
```

### 样例输出 #1

```
550
```

## 提示

对于 $100\%$ 的数据，$h,t,h_i,t_i \le 400$，$n \le 50$，$k_i \le 500$。

# 题解

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
#include<map>
#include<list>
#include<set>
#include<vector>
#include <string.h>
#include<limits.h>
#define ll long long 
#define ull unsigned long long
using namespace std;
const int N = 405;
int m, m1, n, vol1, vol2, val, dp[N][N];
int  main()
{
	ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin >> m >> m1 >> n;
	for (int i = 1; i <= n; i++)//二维费用背包
	{
		cin >> vol1 >> vol2 >> val;
		for (int j = m; j >= vol1; j--)
			for (int k = m1; k >= vol2; k--)
				dp[j][k] = max(dp[j][k], dp[j - vol1][k - vol2] + val);
	}
	cout << dp[m][m1] << endl;
	return 0;
}
```

