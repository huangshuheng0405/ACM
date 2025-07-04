# 双向链表(map)

## 数太大到1e9数组开不了那么大,用map,存左右边的数

```c++
#include<iostream>
#include<map>
using namespace std;
int a[200010];
map<int,int>l,r;
int main(){
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i];
    int num=a[1];//记录第一个结点,后面要从第一个结点输出
    for(int i=2;i<=n;i++){
        l[a[i]]=a[i-1];
        r[a[i-1]]=a[i];
    }
    int q,op,x,y;
    cin>>q;
    while(q--){
        cin>>op;
        if(op==1){//把y插到x的右边
            cin>>x>>y;
            r[y]=r[x];
            l[y]=x;
            l[r[x]]=y;
            r[x]=y;
        }
        else{//删除x
            cin>>x;
            l[r[x]]=l[x];//x的后一个结点的前驱指向x的前一个结点
            r[l[x]]=r[x];//x的前一个结点的后继指向x的后一个结点
            if(num==x)//如果删的是第一个结点
                num=r[x];//num指向x后面的结点
        }
    }
    while(num!=0){
        cout<<num<<' ';
        num=r[num];//指向num后面的数
    }
    return 0;
}
```

```c++
#include<iostream>
using namespace std;
struct linklist{//双向链表
    int front,rear;
}l[100005]={0};
bool vis[100005];//记录同学是否在队列中
int main(){
    int n;
    cin>>n;
    for(int i=0;i<=n;i++)
        l[i].rear=l[i].front=0;
    l[0].rear=1;//第一个结点是1
    l[1].front=0;//第一个结点的后继指向空
    vis[1]=true;//第一个同学入队
    for(int i=2;i<=n;i++){
        int k,p;
        cin>>k>>p;
        vis[i]=true;//i号同学入队
        if(p==0){//把i号插到k号的左边
            l[i].rear=k;
            l[l[k].front].rear=i;
            l[i].front=l[k].front;
            l[k].front=i;
        }
        else{//把i插在k的右边
            l[i].front=k;
            l[l[k].rear].front=i;
            l[i].rear=l[k].rear;
            l[k].rear=i;

        }
    }
    int m;
    cin>>m;
    while(m--){
        int x;
        cin>>x;
        if(vis[x]==false)//x号同学不在队列就跳过
            continue;
        vis[x]=false;//x号同学出队
        l[l[x].front].rear=l[x].rear;//x的前一个结点的后继指向x后一个结点
        l[l[x].rear].front=l[x].front;//x的后一个结点的前驱指向x的前一个结点
    }
    int num=0;
    while(l[num].rear!=0){//从1号同学开始直到最后一个同学
        cout<<l[num].rear<<' ';
        num=l[num].rear;//相当于指针后移
    }
    return 0;
}
```

