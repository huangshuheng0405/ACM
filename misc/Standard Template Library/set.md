# set

每个元素只出现一次，底层是用二叉树实现的，访问元素的时间复杂度O(logn)

```c++
#include<iostream>
#include<set>
using namespace std;
struct node{
    int a,b;
    bool operator< (const node W)const
    {
        return a>W.a;  //按a的值升序 
    }
};
//set<node>s;
int main(){
    set<int,less<int>>q;    //升序排列
    set<int,greater<int>>p;  //降序排列
    int x=0;
    q.insert(x);	//将x插入q中
    q.erase(x);		//删除q中的x元素,返回0或1,0表示set中不存在x
    q.clear();		//清空q
    q.count();      //查找set中某个键值出现的次数，只可能是0或1用处不大
    q.empty();		//判断q是否为空，若是返回1，否则返回0
    q.size();		//返回q中元素的个数
    q.find(x);		//在q中查找x，返回x的迭代器，若x不存在，则返回指向q尾部的迭代器即 q.end()
    q.lower_bound(x); //返回一个迭代器，指向第一个键值大于等于x的元素
    q.upper_bound(x); //返回一个迭代器，指向第一个键值大于x的元素
    q.rend();		  //返回第一个元素的的前一个元素迭代器
    q.begin();		  //返回指向q中第一个元素的迭代器
    q.end();		 //返回指向q最后一个元素下一个位置的迭代器
    q.rbegin();		 //返回最后一个元素
    set<int>::iterator i;//迭代器 相当于指针只能通过迭代器访问
    for(i=q.begin();i!=q.end();i++)//set的遍历
        cout<<*i<<' ';
    //用auto可以免去麻烦的声明
    for(auto i=s.begin();i!=s.end();i++) {
        cout<<*i<<' ';
    }
}
```

### erase

`s.erase(it)`，it为要删除元素的迭代器，时间复杂度$O(1)$

`s.erase(value)`，value为要删除的元素时间复杂度为$O(logN)$,$N$是set的元素个数

`s.erase(start,end)`，start为要删除区间的其实迭代器，end为删除区间结束迭代器的下一个地址

# unordered_set

需包含头文件`unordered_set`

元素无序且**只能出现一次**

### insert

从第一个位置插入

```c++
void solve()
{
    unordered_set<int> s;
    for (int i = 1; i <= 5;i++){
        s.insert(i);
    }
    for(auto i:s){
        cout << i << ' ';
    }
}
result:5 4 3 2 1
```

# multiset

元素从小到大（默认）**排序**且元素可以出现**多次**

```c++
multiset<int，greater<int>>s;//从大到小排序
```

# unordered_multiset