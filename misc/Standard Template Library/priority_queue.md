# 优先队列

内部维护了一个堆，只能从对头取元素

默认从大到小排列

如果是pair类型则按第一个第一个值进行排序

```c++
#include<iostream>
#include<queue>
using namespace std;
//greater从小到大排，less从大到小排
priority_queue<int,vector<int>,greater<int>>q;
int main(){
    int Size=q.size();
    bool flag=q.empty();//空返回true
    q.push(1);
    q.pop();//删除第一个元素
    int num=q.top();//返回第一个元素的值
}
```

sort排序是greater是降序而优先队列是升序，与之相反

### 自定义排序

法一：运算符重载

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#define PP pair<int,int>

struct Node{

    int x,y,z;
    Node(int a,int b,int c)
    {
        x=a,y=b,z=c;
    }
    bool operator<(const Node &a) const   //*****  运算符重载 < ,内置
    {
        return a.z>z;  //按z值降序
    }
};

int main()
{
    priority_queue<Node> q;
    q.push(Node(1,2,3));
    q.push(Node(3,9,1));
    q.push(Node(2,3,4));
    while(!q.empty())
    {
        printf("%d %d %d\n",q.top().x,q.top().y,q.top().z);
        q.pop();
    }

    return 0;
}


```

法二：重写仿函数

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#define PP pair<int,int>

struct cmp{    
            //重写仿函数
   bool operator()(PP a,PP b)
   {
       return a.first>b.first;
   }

};

int main()
{
     priority_queue<PP,vector<PP>,cmp> q;

     q.push(make_pair(4,6));
     q.push(make_pair(9,4));
     q.push(make_pair(6,1));
     while(!q.empty())
     {
         printf("%d %d\n",q.top().first,q.top().second);
         q.pop();
     }

    return 0;
}


```

