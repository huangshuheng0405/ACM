# 快速幂

```c++
#include <iostream>
#include <stdio.h>
#include<math.h>
#include<stdlib.h>
#include<algorithm>
#include<stack>
#include<queue>
#include<deque>
#include<iomanip>
#include <string>
#include<limits.h>
using namespace std;
long long int a, b, p;
int main()
{
	cin >> a >> b >> p;
	long long int ans = 1;
	long long int num = a, num1 = b;
	while (num1 > 0)
	{
		if (num1 & 1)//进行与运算，如果是1，ans乘上相应的数
		{
			ans *= num;
			ans %= p;
		}
		num *= num;
		num %= p;
		num1 = num1 >> 1;//向右移一位
	}
	printf("%d^%d mod %d=%d", a, b, p, ans);
	return 0;
}
```



