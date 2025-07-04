# lambda

### 排序

```c++
vector<int> a = {5, 4, 3, 2, 1};
  sort(a.begin(), a.end(), [](int x, int y) { return x > y; });
  for (auto i : a) {
    cout << i << ' ';
  }
```

### 包装器

在C++中lambda表达式并不是可递归的，除非你将其指定为`std::function`

需要包含头文件`#include<functional>`

```c++
function<Ret(Args...)>;
//function<返回类型(参数类型1，参数类型2)>

    function<void(int)> dfs = [&](int u) {
        vis[u] = true;
        for (auto v : adj[u]) {
            if (!vis[v]) {
                dfs(v);
                fa[v] = u; // 回到u时 v的父节点指向u
            }
        }
        for (auto [v, i] : query[u]) { // 离开u时 枚举查询
            if (vis[v]) {
                ans[i] = find(find, v);
            }
        }
    };
```

Ret：被调用函数的返回类型

Args...：被调用函数的形参

### 直接传入

不能这样定义

```c++
vector<int>adj[n+1];//lambda递归会报错
```

这样才不会报错

```c++
vector<vector<int>>adj(n+1);

vector<vector<int>>adj;
adj.assign(n+1,{});
```

