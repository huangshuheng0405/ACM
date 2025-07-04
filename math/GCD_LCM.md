对于C++17，我们可以使用$<numeric>$头中的`std::gcd`和`std::lcm` 求最大公约数和最小公倍数

# 最大公约数

Greatest Common Divisor，缩写为gcd

### 欧几里得算法（Euclidean algorithm）

$gcd(x,y)=gcd(x,y-x)$

```c++
// Version 1
int gcd(int a, int b) {
  if (b == 0) return a;
  return gcd(b, a % b);
}

// Version 2
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
```

如果两个数$a$和$b$满足$gcd(a,b)=1$，称$a$和$b$互质

### 性质

##### 1.gcd(a,b)=gcd(b,a)

##### 2.gcd(-a,b)=gcd(a,b)

##### 3.gcd(a,0)=|a|

##### 4.gcd(a,1)=1

##### 5.gcd(a,b)=gcd(b,a mod b)

##### 6.gcd(a,b)=gcd(b,a-b)（a如果很大b很小就可以将a一直减b等价于a%b）

##### 附加一个自然数m，则满足

##### 7.gcd(ma,mb)=m*gcd(a,b)

##### 8.gcd(a+mb,b)=gcd(a,b)

##### 如果m是两数的最大公约数

##### 9.gcd(a/m,b/m)=gcd(a,b)/m

# 最小公倍数

Least Common Multiple

### 公式法

$$
a*b=GCD*LCM
$$

### 分解质因子法

先把这几个数的质因数写出来，最小公倍数等于他们所有的质因数的乘积（如果有几个质因数相同，则比较两个数中哪个数有该质因数的个数比较多，乘比较多的次数）

例如45=3x3x5，

# 欧几里得算法（Extended Euclidean algorithm,EXGCD）

### gcd(a,b)=gcd(b,a%b)

```c++
int gcd(int a,int b){
    return b==0?a:gcd(b,a%b);
}
```

# 扩展欧几里得算法

#### Extended Euclidean algorithm,EXGCD

常用于求$ax+by=gcd(a,b)$的一组可行解

```c++
void exgcd(int a, int b, int& x, int& y) {
  if (b == 0) {
    x = 1, y = 0;
    return;
  }
  exgcd(b, a % b, y, x);
  y -= a / b * x;
}
```







