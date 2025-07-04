# 拓展KMP

## 洛谷P5410 求最长公共子序列

```c++
#include<iostream>
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
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
#include<string.h>
#include<string>
#include<limits.h>
#include<unordered_map>
#define ll long long 
#define ull unsigned long long
#define endl '\n'
using namespace std;
int dx[4] = { 0,0,1,-1 };
int dy[4] = { 1,-1,0,0 };
const int INF = 0x3f3f3f3f;
const int max_n = 2e7 + 10;
char a[max_n], b[max_n];
int s[max_n], z[max_n];
void exkmp_z(char* a, int length) {
	s[1] = length;
	for (int i = 2, l, r = 0; i <= length; i++) {
		//在盒内
		if (i <= r)s[i] = min(s[i - l + 1], r - i + 1);
		//从字符串开头和当前位置开始枚举有相同的盒子长度加一
		while (a[1 + s[i]] == a[i + s[i]])s[i]++;
		//当前位置的长度大于右边界 更新盒子左右边界
		if (i + s[i] - 1 > r)l = i, r = i + s[i] - 1;
	}
}
void exkmp_p(char* a, char* b, int length_a, int length_b) {
	for (int i = 1, l, r = 0; i <= length_a; i++) {
		if (i <= r)z[i] = min(s[i - l + 1], r - i + 1);
		while (1 + z[i] <= length_b && i + z[i] <= length_a && b[1 + z[i]] == a[i + z[i]])z[i]++;
		if (i + z[i] - 1 > r)l = i, r = i + z[i] - 1;
	}
}
int  main()
{
	ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin >> a + 1 >> b + 1;
	int n = strlen(a + 1), m = strlen(b + 1);
	exkmp_z(b, m);
	exkmp_p(a, b, n, m);
	ll ans_1 = 0, ans_2 = 0;
	for (int i = 1; i <= m; i++)ans_1 ^= (ull)i * (s[i] + 1);
	for (int i = 1; i <= n; i++)ans_2 ^= (ull)i * (z[i] + 1);
	cout << ans_1 << endl << ans_2;
	return 0;
}
```

