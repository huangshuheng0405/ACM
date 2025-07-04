# map

### 内部是红黑树，具有自动排序功能，默认是按照键值来排序，默认是升序

### 如果map有这个关键字用insert是插入不了的，但用数组就可以覆盖以前关键字对应的值

```c++
#include<iostream>
#include<map>
using namespace std;
int main(){
    map<int,int>m;//key-value
    //pair插入
    m.insert(pair<int,int>(1,10));
    //数组插入
    m[2]=10;
    //查找关键字对应的值(find)
    auto it=m.find(1);//没找到返回的是m.end
    if(it!=m.end())
        cout<<it->first<<it->second<<endl;//输出key对应的值
    else
        cout<<"not find";
    //用迭代器删除
    //查找关键字(count)
    int cnt=m.count(x);//查找x出现的次数,但是键值是唯一的所以要么是1,要么没出现过为0
    auto s=m.find(1);
    m.erase(s);
    //用关键字删除
    int n=m.erase(1);//删除成功返回1否则返回0
    //迭代器范围删除
    m.erase(m.begin(),m.end());
    //清空map
    m.clear();
    //map的大小
    int mSize=m.size();

}
```

### map自定义排序

```c++
struct cmp
{
  bool operator()(int x, int y)
  {
    return x > y;
  }
};
void solve()
{
  map<int, int, cmp> mp;
  for (int i = 0; i < 10; i++)
  {
    mp[i] = 100 + i;
  }
  for (auto i = mp.begin(); i != mp.end(); i++)
  {
    cout << i->first << ' ' << i->second << endl;
  }
}
```



# unordered_map

### 查找速度更快,内部是哈希表

### 头文件#include<unordered_map>