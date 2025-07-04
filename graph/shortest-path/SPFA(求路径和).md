# SPFA(求路径和)

## 洛谷P1828 图可能不连通,答案要注意溢出

```c++
#include<iostream>
#include<queue>
#include<vector>
#include<string.h>
#include<algorithm>
using namespace std;
const int Maxsize=1500;
const int INF=0x3f3f3f3f;//图可能不连通 INF太大ans可能溢出 可以把res和ans定义为ll
struct edge{
    int v,w;
};
vector<edge>e[Maxsize];
bool vis[Maxsize];
int dis[Maxsize],n,p,c,cow[605];
queue<int>q;
void SPFA(int s){
    memset(vis,0,sizeof(vis));
    for(int i=1;i<=p;i++)
        dis[i]=INF;
    dis[s]=0;//s到本身的路径为0
    vis[s]=true;
    q.push(s);
    while(!q.empty()){
        int u=q.front();
        q.pop();
        vis[u]=false;//u点出队
        for(auto ed:e[u]){//枚举u的邻点
            int v=ed.v,w=ed.w;
            if(dis[v]>dis[u]+w){
                dis[v]=dis[u]+w;
                if(vis[v]!=true){
                    q.push(v);//v点距离变小才能去更新其他点
                    vis[v]=1;//v点入队
                }
            }
        }
    }
}
int main(){
    cin>>n>>p>>c;
    for(int i=1;i<=n;i++)
        cin>>cow[i];
    for(int i=1;i<=c;i++){
        int u,v,w;
        cin>>u>>v>>w;
        //无向边
        e[v].push_back({u,w});
        e[u].push_back({v,w});
    }
    long long res=1e9;
    for(int i=1;i<=p;i++){
        //从i点开始做SPFA就是把黄油放到i牧场,求出其他点就到i牧场的距离
        SPFA(i);//每个点都要进行一次SPFA
        long long ans=0;
        for(int j=1;j<=n;j++)
            ans+=dis[cow[j]];//求出路径和
        res=min(res,ans);//更新最小答案
    }
    cout<<res;
    return 0;
}
```

