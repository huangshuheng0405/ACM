# 树链剖分

**树链剖分**有多种形式，如重链剖分，长链剖分和用于Link/cut Tree的剖分，大多数情况下（没有特殊说明时），树链剖分都指**重链剖分**

### 重子结点

表示节点中子树最大的子结点。如过有多个子树最大的子结点，取其一。如果没有子结点，就无重子结点

### 轻子节点

除了重子结点以外的其他结点

### 轻边

到其他轻子结点的边为**轻边**

### 重边

从这个结点到重子结点的边为**重边**

### 重链

若干条首位衔接的重边构成**重链**

# 实现

树剖的实现分为两个DFS，第一次DFS求出$fa(x),dep(x),siz(x),son(x)$，第二次DFS求出$top(x),dfn(x),nw(x)$

- $fa(x)$表示结点$x$在树上的父亲
- $dep(x)$表示结点$x$在树上的深度
- $siz(x)$表示结点$x$的子树的结点个数
- $son(x)$表示结点$x$的**重儿子**
- $top(X)$表示结点$x$所在**重链**的顶部结点（深度最小）
- $dfn(x)$表示结点$x$的**DFS序**，也是其在线段树中的编号
- $nw$表示新编号在树中所对应的结点的权值

# 【模板】重链剖分/树链剖分

## 题目描述

如题，已知一棵包含 $N$ 个结点的树（连通且无环），每个节点上包含一个数值，需要支持以下操作：

- `1 x y z`，表示将树从 $x$ 到 $y$ 结点最短路径上所有节点的值都加上 $z$。

- `2 x y`，表示求树从 $x$ 到 $y$ 结点最短路径上所有节点的值之和。

- `3 x z`，表示将以 $x$ 为根节点的子树内所有节点值都加上 $z$。

- `4 x` 表示求以 $x$ 为根节点的子树内所有节点值之和

## 输入格式

第一行包含 $4$ 个正整数 $N,M,R,P$，分别表示树的结点个数、操作个数、根节点序号和取模数（**即所有的输出结果均对此取模**）。

接下来一行包含 $N$ 个非负整数，分别依次表示各个节点上初始的数值。

接下来 $N-1$ 行每行包含两个整数 $x,y$，表示点 $x$ 和点 $y$ 之间连有一条边（保证无环且连通）。

接下来 $M$ 行每行包含若干个正整数，每行表示一个操作。

## 输出格式

输出包含若干行，分别依次表示每个操作 $2$ 或操作 $4$ 所得的结果（**对 $P$ 取模**）。

## 样例 #1

### 样例输入 #1

```
5 5 2 24
7 3 7 8 0 
1 2
1 5
3 1
4 1
3 4 2
3 2 2
4 5
1 5 1 3
2 1 3
```

### 样例输出 #1

```
2
21
```

## 提示

**【数据规模】**

对于 $30\%$ 的数据： $1 \leq N \leq 10$，$1 \leq M \leq 10$；

对于 $70\%$ 的数据： $1 \leq N \leq {10}^3$，$1 \leq M \leq {10}^3$；

对于 $100\%$ 的数据： $1\le N \leq {10}^5$，$1\le M \leq {10}^5$，$1\le R\le N$，$1\le P \le 2^{30}$。所有输入的数均在 `int` 范围内。

**【样例说明】**

树的结构如下：

![](https://cdn.luogu.com.cn/upload/pic/2319.png)

各个操作如下：

![](https://cdn.luogu.com.cn/upload/pic/2320.png)

故输出应依次为 $2$ 和 $21$。

# 题解

利用**线段树**来维护区间和以及区间修改

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;

#define LL long long
#define lc u<<1
#define rc u<<1|1
const int N=100010;
int n,m,a,b,root,P,w[N];
vector<int> e[N];
int fa[N],son[N],dep[N],siz[N];
int top[N],dfn[N],nw[N],tim; //重链
struct tree{
  int l,r;
  LL add,sum;
}tr[N*4]; //线段树

void dfs1(int u,int f){//搞fa,dep,siz,son
  fa[u]=f,dep[u]=dep[f]+1,siz[u]=1;
  for(int v:e[u]){
    if(v==f) continue;
    dfs1(v,u);
    siz[u]+=siz[v];
    if(siz[son[u]]<siz[v]) son[u]=v;
  }
}
void dfs2(int u,int tp){ //搞top,id,nw
  top[u]=tp,dfn[u]=++tim,nw[tim]=w[u];
  if(!son[u]) return;
  dfs2(son[u],tp);
  for(int v:e[u]){
    if(v==fa[u]||v==son[u])continue;
    dfs2(v,v);
  }
}
void pushup(int u){
  tr[u].sum=tr[lc].sum+tr[rc].sum;
}
void pushdown(int u){
  if(tr[u].add){
    tr[lc].sum+=tr[u].add*(tr[lc].r-tr[lc].l+1);
    tr[rc].sum+=tr[u].add*(tr[rc].r-tr[rc].l+1);
    tr[lc].add+=tr[u].add;
    tr[rc].add+=tr[u].add;
    tr[u].add=0;
  }
}
void build(int u,int l,int r){ //构建线段树
  tr[u]={l,r,0,nw[r]};
  if(l==r) return;
  int mid=l+r>>1;
  build(lc,l,mid),build(rc,mid+1,r);
  pushup(u);
}
void change(int u,int l,int r,int val){ //线段树修改
  if(l<=tr[u].l&&tr[u].r<=r){
    tr[u].add+=val;
    tr[u].sum+=val*(tr[u].r-tr[u].l+1);
    return;
  }
  pushdown(u);
  int mid=tr[u].l+tr[u].r>>1;
  if(l<=mid) change(lc,l,r,val);
  if(r>mid) change(rc,l,r,val);
  pushup(u);
}
void change_path(int u,int v,int val){ //修改路径
  while(top[u]!=top[v]){
    if(dep[top[u]]<dep[top[v]]) swap(u,v);
    change(1,dfn[top[u]],dfn[u],val);
    u=fa[top[u]];
  }
  if(dep[u]<dep[v]) swap(u,v);
  change(1,dfn[v],dfn[u],val); //最后一段
}
void change_tree(int u,int val){ //修改子树
  change(1,dfn[u],dfn[u]+siz[u]-1,val);
}
LL query(int u,int l,int r){ //线段树查询
  if(l<=tr[u].l&&tr[u].r<=r)return tr[u].sum;
  pushdown(u);
  int mid=tr[u].l+tr[u].r>>1;
  LL res=0;
  if(l<=mid) res+=query(lc,l,r);
  if(r>mid) res+=query(rc,l,r);
  return res;
}
LL query_path(int u,int v){ //查询路径
  LL res=0;
  while(top[u]!=top[v]){
    if(dep[top[u]]<dep[top[v]]) swap(u,v);
    res+=query(1,dfn[top[u]],dfn[u]);
    u=fa[top[u]];
  }
  if(dep[u]<dep[v]) swap(u,v);
  res+=query(1,dfn[v],dfn[u]); //最后一段
  return res;
}
LL query_tree(int u){ //查询子树
  return query(1,dfn[u],dfn[u]+siz[u]-1);
}
int main(){
  cin>>n>>m>>root>>P;
  for(int i=1; i<=n; i++) cin>>w[i];
  for(int i=0; i<n-1; i++){
    cin>>a>>b;
    e[a].push_back(b); e[b].push_back(a);
  }
  dfs1(root,0);
  dfs2(root,root); //把树拆成链
  build(1,1,n);  //用链建线段树
  while(m--){
    int t,u,v,val; cin>>t>>u;
    if(t==1){
      cin>>v>>val;
      change_path(u,v,val);
    }
    else if(t==3){
      cin>>val;
      change_tree(u,val);
    }
    else if(t==2){
      cin>>v;
      cout<<query_path(u,v)%P<<endl;
    }
    else cout<<query_tree(u)%P<<endl;
  }
}
```

