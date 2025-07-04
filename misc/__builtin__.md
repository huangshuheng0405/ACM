> 这些函数都有对应的unsigned long和unsigned long long，只需要在函数名后面加上l或ll就可以了，比如__builtin_clzll

# __builtin_ffs

返回x的最后一位1是从后向前第几位，比如7368（1110011001000）返回4

```c++
int n = 1;//1
int m = 8;//1000
cout<<__builtin_ffs(n)<<endl;//输出1
cout<<__builtin_ffs(m)<<endl;//输出4

```

# __builtin_clz

**返回前导0的个数**

这个函数作用是返回输入数二进制从最高位开始（左起）的连续的0的个数，如果传入0则行为未定义

```c++
  cout << __builtin_clz(1) << '\n'; // 31
  cout << __builtin_clz(3) << '\n'; // 30
  cout << __builtin_clz(5) << '\n'; // 29
  int n = 6;
  cout << 32 - __builtin_clz(n) << '\n'; //n 的二进制长度
```

# __builtin_ctz

返回后面0的个数，和__builtin_clz相对

这个函数作用是返回输入数二进制表示从最低位开始（右起）的连续0的个数，如果传入0则行为未定义

```c++
int n = 1;//1
int m = 8;//1000
cout<<__builtin_ctzll(n)<<endl;//输出0
cout<<__builtin_ctz(m)<<endl;//输出3
```

# __builtin_popcount

返回二进制表示1的个数，如果传入0的个数则返回0

```c++
int n = 15; //二进制为1111
cout<<__builtin_popcount(n)<<endl;//输出4
```

# __builtin_parity

返回x的奇偶校验位，也就是x的1的个数模2的结果

```c++
int n = 15;//二进制为1111
int m = 7;//111
cout<<__builtin_parity(n)<<endl;//偶数个，输出0
cout<<__builtin_parity(m)<<endl;//奇数个，输出1
```

