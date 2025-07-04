# Tuple

可以保存不同类型的元素，可以通过下标或者`std::get`函数来访问其中的值

```c++
#include <bits/stdc++.h>
using namespace std;

tuple<string, int, int> t[100];

int main() {
    t[1] = {"abcd", 123, 345};
    t[2] = {"abd", 13, 34};

    for (int i = 1; i <= 2; i++) {
        auto [x, y, z] = t[i];
        cout << x << " " << y << " " << z << "\n";
    }

}
```

