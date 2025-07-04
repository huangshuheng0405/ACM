# 整除

#### 定义

设$a,b\in Z,a\neq 0$。如果$\exist q\in Z$，使得$b=ap$，那么就说$b$可被$a$整除，记作$a|b$

# 约数

#### 定义

若$a|b$，则称$b$是$a$的倍数，$a$是$b$的约数

# 同余

#### 定义

设整数$m\neq 0$。若$m|(a-b)$,称$m$为模数（模），$a$同余与$b$模$m$，$b$是$a$对模$m$的剩余。记作$a\equiv b (mod\ m)$。

根据整除的性质，上述同余式也等价于$a\equiv b(mod\ (-m))$

如果两个数$a$和$b$的差能够被$m$整除，那么就说$a$和$b$对模数$m$同余（关于$m$同余）。

#### 同余的性质

同余是**等价关系**，即同余具有

- 自反性：$a\equiv a(mod\ m)$
- 对称性：若$a\equiv b (mod\ m)$，则$b\equiv a(mod\ m)$
- 传递性：若$a\equiv b(mod\ m)$，$b\equiv c (mod\ m)$，$a\equiv c(mod\ m)$

线性运算：若$a,b,c,d\in Z,m\in N^{*},a\equiv b(mod\ m),c\equiv d(mod\ m)$

- $a\pm c\equiv b\pm d(mod\ m)$
- $a\times c\equiv b\times d(mod\ m)$

若$a,b\in Z,k,m,\in N^*,a\equiv b(mod\ m)$成立时，则$ak\equiv bk(mod\ m)$​，即如果ac≡bc(mod m),且c和m互质，则有a≡b(mod m)(就是说同余式两边可以同时除以一个和模数互质的数)。

若$a,b\in Z,d,m\in N^*,d|m$，则当$a\equiv b(mod\ m)$成立时，有$a\equiv b(mod\ m)$