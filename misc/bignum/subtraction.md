# P2142 高精度减法

## 题目描述

高精度减法。

## 输入格式

两个整数 $a,b$（第二个可能比第一个大）。

## 输出格式

结果（是负数要输出负号）。

## 输入输出样例 #1

### 输入 #1

```
2
1
```

### 输出 #1

```
1
```

## 说明/提示

- $20\%$ 数据 $a,b$ 在 long long 范围内；
- $100\%$ 数据 $0<a,b\le 10^{10086}$。

# solve

先比较大小，用大数减小数

删除前导零，保留一位

```c++
#include<bits/stdc++.h>

int main() {
	std::string a, b;
	std::cin >> a >> b;
	auto check = [&](std::string a, std::string b) -> bool {
		if (a.size() != b.size()) {
			return a.size() > b.size();
		}
		for (int i = 0; i < a.size(); i++) {
			if (a[i] != b[i]) {
				return a[i] > b[i];
			}
		}
		return true;
	};
	bool ok = false;
	if (!check(a, b)) {
		std::swap(a, b);
		ok = true;
	}
	reverse(a.begin(), a.end());
	reverse(b.begin(), b.end());
	int len = std::max(a.size(), b.size());
	std::vector<int> A(len + 10), B(len + 10), C(len + 10);
	for (int i = 1; i <= a.size(); i++) {
		A[i] = a[i - 1] - '0';
	}
	for (int i = 1; i <= b.size(); i++) {
		B[i] = b[i - 1] - '0';
	}
	for (int i = 1; i <= len; i++) {
		if (A[i] < B[i]) {
			A[i + 1]--;
			A[i] += 10;
		}
		C[i] = A[i] - B[i];
	}
	while (C[len] == 0 && len > 1) {
		len--;
	} 
	if (ok) {
		std::cout << "-";
	}
	for (int i = len; i > 0; i--) {
		std::cout << C[i];
	}
}
```

