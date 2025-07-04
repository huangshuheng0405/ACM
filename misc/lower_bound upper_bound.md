# lower_bound upper_bound

```c++
upper_bound(begin, end, value)
```

都是algorithm头文件里的，在一个排好序的数组里面进行二分查找，时间复杂度是$O(logN)$

#### 默认从小到大的数组

upper_bound在区间中查找第一个**大于**value的数，找到返回其地址，否则返回**end的地址**

lower_bound在区间中查找第一个**大于等于**value的数

从大到小的数组

```c++
upper_bound(begin,end,value,greater<int>() )
```

upper_bound查找第一个**小于**value的数

lower_bound查找第一个**小于等于**value的数

可自定义排序方法