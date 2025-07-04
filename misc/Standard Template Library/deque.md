# deque

### 头文件

`#include<deque>`

### 定义

```c++
// 1. 定义一个int类型的空双端队列 v0
deque<int> v0;
// 2. 定义一个int类型的双端队列 v1，并设置初始大小为10; 线性复杂度
deque<int> v1(10);
// 3. 定义一个int类型的双端队列 v2，并初始化为10个1; 线性复杂度
deque<int> v2(10, 1);
// 4. 复制已有的双端队列 v1; 线性复杂度
deque<int> v3(v1);
// 5. 创建一个v2的拷贝deque v4，其内容是v4[0]至v4[2]; 线性复杂度
deque<int> v4(v2.begin(), v2.begin() + 3);
// 6. 移动v2到新创建的deque v5，不发生拷贝; 常数复杂度; 需要 C++11
deque<int> v5(std::move(v2));
```

### 方法函数

`push_back(x)\push_front(x)`：把`x`插入队尾、队首

`back()\front()`：返回队尾、队首元素

`pop_back()\pop_front()`：删除队尾、队首元素

`erase(first,last)`：删除队列中`[first,last)`中的元素**左闭右开**

`empty()`：判断队列是否为空

`size()`：返回队列元素的数量

`clear()`：清空deque

deque可以进行排序

```c++
sort(q.begin(),q.end());//从小到大
sort(q.begin(),q.end(),greater<int>());//从大到小
```

