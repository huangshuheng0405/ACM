# 增广路定理

### 定义

- 交错路（alternating path）始于非匹配点且由匹配边与非匹配边交错而成
- 增广路（augmenting path）是始于非匹配点且终于非匹配点的交错路。增广路中边的数量是奇数

增广路上非匹配边比匹配边数量多 1，如果将增广路上的匹配边和未匹配边反转，则匹配数量会增加 1 且依然是交错路。![augment-1](https://oi-wiki.org/graph/graph-matching/images/augment-1.png)

如上图，匹配数从 2 增加为 3，匈牙利算法中只通过这样的方式增加匹配数量，称为 **增广（Augment）**。

根据 Berge's lemma 当找不到增广路的时候，得到最大匹配。