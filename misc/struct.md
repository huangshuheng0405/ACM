### 结构体外重载运算符

```c++
struct node{
	int u,v;
}
bool operator<(const &a,const &b){		
    return a.u<b.u;		
}
```

### 结构体内重载运算符

x是当前的值，u.x是待比较的值

```c++
struct node {
    int x, y;

    bool operator<(const node &u) const { return x > u.x; }//按x降序排列
};
```

