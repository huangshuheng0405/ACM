# next_permutation

如果是最后一个排列返回`false`，否则返回`true`，所以全排列之前要先从小到大排序

```c++
int n;
    cin >> n;
    vector<int> a(n + 1);
    for (int i = 1; i <= n; i++)
    {
        a[i] = i;
    }
    do
    {
        for (int i = 1; i <= n; i++)
        {
            printf("%d ", a[i]);
        }
        cout << endl;
    } while (next_permutation(a.begin() + 1, a.end()));
```

# prev_permutation

求出前一个排列
