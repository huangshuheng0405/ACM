# 向上取整

### 检查余数

```c++
if(temp%k == 0)
	result = temp/k;
else
	result = (temp/k)+1;

```

### 数学思想

```c++
int result = (temp - 1)/k + 1;
//等价于
int result = (temp + k - 1)/k;
//第一句代码中的1换成（k/k）就得到第二句代码了
//可以自己带入数字检验一下

```

### math.h头文件的ceil函数

```c++
//注意！！！
//ceil()函数返回的是double型，所以用强制转换
int result = (int) ceil(temp*1.0/k);

```

# 向下取整

### math.h头文件的floor函数

# 四舍五入

### math.h头文件的round函数

# int

### 屏蔽后面的小数

### 整除‘/’向下取整

# 判断是否为整数

```c++
(a/b)*b+a%b==a
```

