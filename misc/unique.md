# unique

`unique(first,last)`

去重前要先进行sort排序，将[first,last]范围内的相邻重复元素去除，并返回去重后的尾后迭代器去重后的范围中只保留第一个出现的元素，将重复出现的元素移到数组后面

```c++
sort(a,a+5);//数组
int pos = unique(a,a+5);//unique去重返回尾地址pos
erase(pos,a+5);//从尾地址到最后空间进行删除
-------------------------------------------------
sort(vec.begin(),vec.end());//vector
int pos = unique(vec.begin(), vec.end());//unique去重返回尾地址pos
vec.erase(pos, vec.end());//从尾地址到最后空间进行删除
```

