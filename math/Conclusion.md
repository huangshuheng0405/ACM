直接暴力做法的时间复杂度是$O(N^2)$,因为要枚举所有的$i<j$ 的组合，但通过**数学变形**有$O(N)$的做法
$$
\sum_{i \leq i<j\leq N}A_iA_j=\frac{1}{2}\left[\left(\sum^N_{i=1}A_i\right)^2-\sum^N_{i=1}A_i^2\right]
$$
求出总和、平方和,套公式即可





两个质数之间的间隔很小



计算斜率
$$
\frac{y_2 -y_1}{x_2-x_1}
$$
但是会出现除数是零的情况，可以用**叉积**来判断是否共线
$$
a\times b=x_1y_2-x_2y_1
$$
如果$a\times b$为零，那么$a$与$b$共线



判断一个数是否能被$8$整除

对于大于$1000$的数，因为$1000$能被$8$整除，所以只需要判断后三位是否能被$8$整除即可，对于小于$1000$的数来说，正常对$8$取模

```c++
	std::vector<int> a(10);
    for (auto i : s) {
    	a[i - '0']++;
    }
    for (int i = 112; i < 1000; i += 8) {
  		auto tmp = a;
  		for (char x : std::to_string(i)) {
  			tmp[x - '0']--;
  		}  		
  		if (std::all_of(tmp.begin(), tmp.end(), [&](int x) {// 保证每个数都有
  			return x >= 0;
  		})) {
  			std::cout << "Yes";
  			return 0;
  		}
    }
	std::cout << "No";
	return 0;
```

