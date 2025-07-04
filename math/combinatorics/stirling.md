# 第二类斯特林数

# Stirling Number

![](C:\Users\黄书恒1\Pictures\Code\第二类斯特林数.png)

## 盒子是互不区分的！！！

```c++
void calc(){
    s[0][0]=1;//边界
    for(int i=1;i<=10;i++)
        for(int j=1;j<=10;j++)
            s[i][j]=s[i-1][j-1]+j*s[i-1][j];//第二类斯特林数递推式
}
```

