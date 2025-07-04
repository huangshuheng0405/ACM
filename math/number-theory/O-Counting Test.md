# O-Counting Test

`cnt`数组记录每个位置的每个字母出现的次数

答案就是前缀和做差$cnt[r]-cnt[l-1]$，注意：左右端点可能都会比长度$n$长

```c++
#include <bits/stdc++.h>

using i64 = long long;

void solve() {
	int n, q;
	std::cin >> n >> q;

	std::string s;
	std::cin >> s;

	std::vector<std::array<int, 26>> cnt(n + 1);
	for (int i = 1; i <= n; i++) {
		cnt[i] = cnt[i - 1];
		cnt[i][s[i - 1] - 'a']++;
	}

	while (q--) {
		int l, r;
		char c;

		std::cin >> l >> r >> c;
		l--;

		int x = cnt[n][c - 'a'];    
		int t1 = r / n * x + cnt[r % n][c - 'a'];
		int t2 = l / n * x + cnt[l % n][c - 'a'];
		std::cout << t1 - t2 << "\n";
	}
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(0);

    int t;
    std::cin >> t;

    while (t--) {
    	solve();
    }


    return 0;
}
```

