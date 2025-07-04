# 双向bfs

 昨天晚上，小二日月做了一个可怕的噩梦。他梦见他和他的女朋友分别被困在一个大迷宫里。更可怕的是，迷宫里有两个幽灵。他们会杀死人民。现在小二月天想知道他是否能在鬼魂找到他们之前找到他的女朋友。 

 你可以想象小二月天和他的女朋友可以向四个方向移动。每秒钟，小二日月可以移动三步，他的女朋友可以移动一步。鬼是邪恶的，每一秒他们会分成几个部分，以占领网格内2步到他们，直到他们占领整个迷宫。你可以假设每一秒鬼都是先分开，然后小二月天和他的女朋友开始移动，如果小二月天和他的女朋友和鬼一起到达一个格子，他们就会死。 

 输入以整数T开始，表示测试用例的数量 

 每个测试用例以一行开始，其中包含两个整数n和m，表示迷宫的大小。 

 接下来的n行描述了迷宫。每行包含m个字符。字符可以是： 

 ‘.’表示一个空的地方，所有人都可以走。 

 “X”表示一堵墙，只有人不能在上面走。 

 “M”代表小二月天 

 “G”是指女朋友 

  “Z”表示鬼魂。 

 它保证只包含一个字母M，一个字母G和两个字母Z。 

 在一行中输出一个整数S，表示二月天和他的女朋友如果能成功见面，将在最短时间S内见面，或者输出-1表示他们未能见面。 

```
3
5 6
XXXXXX
XZ..ZX
XXXXXX
M.G...
......
5 6
XXXXXX
XZZ..X
XXXXXX
M.....
..G...

10 10
..........
..X.......
..M.X...X.
X.........
.X..X.X.X.
.........X
..XX....X.
X....G...X
...ZX.X...
...Z..X..X
```

```
1
1
-1
```

# 题解

```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
#define x first
#define y second
using namespace std;

const int N=810;
int n,m;
char g[N][N];  //地图
int vis[N][N]; //2表示女孩走过,1表示男孩走过,0都没走过
pair<int,int> boy,girl,ghost[2]; //存储人、鬼的初始位置
int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1}; //搜索的方向数组

bool check(int x,int y,int tim){
  if(x<0||x>=n||y<0||y>=m||g[x][y]=='X') return false;
  for(int i=0;i<2;i++)
    if(abs(x-ghost[i].x)+abs(y-ghost[i].y)<=tim*2)return false;
  return true; //(x,y)合法则返回true
}
int bfs(){
  int tim=0;
  memset(vis,0,sizeof vis);
  queue<pair<int,int>> qb,qg;
  qb.push(boy); qg.push(girl);
  while(qb.size()||qg.size()){
    tim++; //增加1秒
    for(int i=0;i<3;i++) //男孩走3步
      for(int j=0,s=qb.size();j<s;j++){ //枚举队中所有点
        pair<int,int> t=qb.front(); qb.pop();
        int x=t.x, y=t.y;
        if(!check(x,y,tim)) continue;  //(x,y)非法则跳过
        for(int k=0;k<4;k++){          //4个方向
          int a=x+dx[k], b=y+dy[k];
          if(check(a,b,tim)){            //(a,b)合法
            if(vis[a][b]==2) return tim; //2表示女孩走过
            if(!vis[a][b]) vis[a][b]=1,qb.push({a,b});
          }
        }
      }
    for(int i=0;i<1;i++) //女孩走1步
      for(int j=0,s=qg.size();j<s;j++){
        pair<int,int> t=qg.front();qg.pop();
        int x=t.x, y=t.y;
        if(!check(x,y,tim)) continue;
        for(int k=0;k<4;k++){
          int a=x+dx[k], b=y+dy[k];
          if(check(a,b,tim)){
            if(vis[a][b]==1) return tim; //1表示男孩走过
            if(!vis[a][b]) vis[a][b]=2,qg.push({a,b});
          }
        }
      }
  }
  return -1; //无解返回-1
}
int main(){
  int T;scanf("%d",&T);
  while(T--){
    scanf("%d%d",&n,&m);
    for(int i=0;i<n;i++) scanf("%s",g[i]);
    for(int i=0,t=0;i<n;i++) //找出人、鬼的位置
      for(int j=0;j<m;j++)
        if(g[i][j]=='M') boy={i,j};
        else if(g[i][j]=='G') girl={i,j};
        else if(g[i][j]=='Z') ghost[t++]={i,j};    
    printf("%d\n",bfs());
  }
}
```

