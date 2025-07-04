# 离散化

有些数据因为本身很大或者类型不支持，自身无法作为数组的下标来方便的处理，而影响最终结果的只有元素之间的相对大小关系时或者出现的先后顺序有关，我们可以将原来的数据按照排名来处理问题，即为离散化。对象可以是大整数、浮点数、字符串等

对于原数组中的元素有重复时，一般把相同的元素转化为相同的数据

#### 过程如下

1. 创建原数组的副本
2. 将副本中的值从小到大排序
3. 将排序好的副本去重
4. 查找原数组的每一个元素在副本中的位置，位置即为排名，将其作为离散化后的值

```c++
//arr[i]为初始数组，下标从1到n
for(int i=1;i<=n;i++)
    tmp[i]=arr[i];
sort(a+1,a+1+n);
int len=unique(tmp+1,tmp+1+n)-(tmp+1);//去重
for(int i=1;i<=n;i++)
    arr[i]=lower_bound(tmp+1,tmp+1+len,arr[i])-tmp;
```

我们也可以对`std::vector`进行离散化

```c++
vector<int>arr,tmp;
tmp=arr;
sort(tmp.begin(),tmp.end());
tmp.erase(unique(tmp.begin(),tmp.end()),tmp.end());
for(int i=0;i<n;i++)
    arr[i]=lower_bound(tmp.begin(),tmp.end(),arr[i])-tmp.begin();
```

### 有时候会把相同的元素根据输入顺序离散化为不同的数据

```c++
struct Data{
    int idx,val;
    bool operator <(const Data& x)const{
        if(val==x.val)
            return idx<x.idx;//当值相同时，先出现的元素离散化后的元素更小
        return val<x.val;
    }
}tmp[maxn];
for(int i=1;i<=n;i++)
  	tmp[i]={i,arr[i]};
sort(tmp+1,tmp+1+n);
for(int i=1;i<=n;i++)
    arr[tmp[i].idx]=i;
```

