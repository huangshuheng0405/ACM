# Nim 游戏

$n$ 堆物品，每堆有$a_i$个，两个玩家轮流取走任意一堆的任意个物品，但不能不取，取走最后一个物品的人获胜

# 将 Nim 游戏转换为有向图游戏

## Nim和

定义Nim和$= a_1\oplus a_2\oplus\dots \oplus a_n$

当且仅当 Nim 和为$0$时，该状态为必败状态；否则该状态为必胜状态。

我们可以将一个有 x 个物品的堆视为节点 x，则当且仅当有 y<x 时，节点 x 可以到达 y。

那么，由 n 个堆组成的 Nim 游戏，就可以视为有 n 个有向图游戏。

根据上面的推论，可以得出 SG(x)=x，再根据 SG 定理，就可以得出 Nim 和的结论了

# 【模板】Nim 游戏

## 题目描述

甲，乙两个人玩 nim 取石子游戏。

nim 游戏的规则是这样的：地上有 $n$ 堆石子（每堆石子数量小于 $10^4$），每人每次可从任意一堆石子里取出任意多枚石子扔掉，可以取完，不能不取。每次只能从一堆里取。最后没石子可取的人就输了。假如甲是先手，且告诉你这 $n$ 堆石子的数量，他想知道是否存在先手必胜的策略。

## 输入格式

**本题有多组测试数据。**

第一行一个整数 $T$ （$T\le10$），表示有 $T$ 组数据

接下来每两行是一组数据，第一行一个整数 $n$，表示有 $n$ 堆石子，$n\le10^4$。

第二行有 $n$ 个数，表示每一堆石子的数量.

## 输出格式

共 $T$ 行，每行表示如果对于这组数据存在先手必胜策略则输出 `Yes`，否则输出 `No`。

## 样例 #1

### 样例输入 #1

```
2
2
1 1
2
1 0
```

### 样例输出 #1

```
No
Yes
```