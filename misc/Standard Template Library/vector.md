# vector

#### 动态数组

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int main(){
    vector<int>a(10);//定义一个具有10个元素的容器
    std::vector<int> a{0, -1, -1, -1, -1, -1, -1};//设定初始值
    vector<int>b(10,1);//定义一个具有10个值为1的元素的容器
    vector<int>c(b);//c的值完全等于b
    int num=b.back();//返回b的最后一个元素
    int num1=a.front();//返回a的第一个元素，当且仅当a不为空
    a.clear();//清空a中的元素
    bool flag=a.empty();//a为空返回true
    a.erase(a.begin(),a.begin()+3);//删除a中一个到第二个
    a.pop_back();//删除a的最后一个元素
    a.push_back(1);//在a的最后一个位置插入1
    a.insert(a.begin()+1,3,5);//在a的第一个元素后插入三个数5
    int Size=a.size();//返回a的长度
    vector<int>::iterator it;//迭代器
    sort(a.begin(),a.end());//将a中的元素进行排序
    find(a.begin(),a.end(),10);//在a中查找值为10的位置，返回的是地址,没找到返回a.end()
}
```

### iota

用于填充一个区间，以递增的方式给每个元素赋值。

**必须包含头文件`numeric`**

```c++
#include <iostream>
#include <numeric>
#include <vector>

int main() {
    std::vector<int> nums(5);

    std::iota(nums.begin(), nums.end(), 1);

    for (const auto& num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}

```

输出

```
1 2 3 4 5
```

字符类型也可以

```c++
#include <iostream>
#include <numeric>
#include <list>

int main() {
    std::list<char> chars(4);

    std::iota(chars.begin(), chars.end(), 'A');

    for (const auto& ch : chars) {
        std::cout << ch << " ";
    }
    std::cout << std::endl;

    return 0;
}

```

输出

```
A B C D
```

### insert

`a.insert(a.begin()+1,9)`在数组下标1的位置插入9，原本1位置的数往后移

```c++
#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <string>
#include <vector>
using namespace std;
#define ll long long
#define ull unsigned long long
#define db double
#define lb long double
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
// int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左

void solve() {
  vector<int> a(10);
  for (int i = 0; i <= 5; i++) {
    a[i] = i;
  }
  a.insert(a.begin() + 1, 9);
  for (int i = 0; i <= 5; i++) {
    cout << a[i] << ' ';
  }
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  int _ = 1;

  // cin >> _;
  while (_--) {
    solve();
  }
  return 0;
}
```

##### result

```c++
原数组：0 1 2 3 4 5
后数组：0 9 1 2 3 4
```

### erase

`a.erase(start,end)`括号中表示要删除的长度，如果没有end，默认只删除start位置的数，后面的数往前移

```c++
#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <string>
#include <vector>
using namespace std;
#define ll long long
#define ull unsigned long long
#define db double
#define lb long double
#define endl '\n'
const int N = 1e5 + 5;
const int mod = 1e9 + 7;
const ll inf = 1e18;
// int d[4][2] = {-1, 0, 0, 1, 1, 0, 0, -1};//  上右下左

void solve() {
  vector<int> a(10);
  for (int i = 0; i <= 5; i++) {
    a[i] = i;
  }
  a.erase(a.begin() + 1, a.begin() + 3);
  for (int i = 0; i <= 5; i++) {
    cout << a[i] << ' ';
  }
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  int _ = 1;

  // cin >> _;
  while (_--) {
    solve();
  }
  return 0;
}
```

##### result

```
原数组：0 1 2 3 4 5
后数组：0 3 4 5 0 0 
```

### sort

`sort(start,end,自定义排序方法)`

从小到大排

```c++
vector<int>a(n);
sort(a.begin(),a.end());
```

从大到小

```
vector<int>a(n);
sort(a.begin(),a.end(),greater<int>() )
```

### accumulate

`accumulate(start,end,起始值)`，求区间内的和

```c++
#include <numeric>
 
vector<int> v;
accumulate(v.begin(),v.end(),0);
```

### max_element min_element

`max_element(firsr,last)`，传入参数为两个地址或迭代器，时间复杂度为$O(n)$,$n$为数组大小

```c++
vector<int> a = {10, 11, 234, 54353, 12321};
cout << *max_element(a.begin(), a.end());
```

### nth_element

`nth_element(begin,nth,en)`

### max min

`max(x,y)`返回x和y之间最大的一个

`max({a,b,c,d})`返回a、c、b、d之间最大的一个

```c++
cout << max({1, 2, 34, 5, 334});
```

### binary_search

在已排序的数组或容器里查找特定的元素，通过二分查找算法来找

```c++
vector<int> numbers = {1, 3, 5, 7};

int a = 5;

//使用binary_ search查找目标元素

bool found = binary_ search(numbers.begin(), numbers.end(),a);

if (found)

      {

        cout << "a element " << a << "found." << endl;

     }

        else 

          {

            cout << "a element "<< a << " not found." << endl;

         }

```

### 二维数组的定义

```c++
vector<vector<int>> a(10, vector<int>(10));
```

### reserve

1. `reserve` 函数用于预留容器中的元素数量，但并不改变容器中的元素数量。也就是说，调用` reserve` 后，容器的大小（`size()`）不会改变，但容量（`capacity()`）会增加到或超过指定的值。
2. 使用` reserve `可以减少因插入新元素而导致的内存重新分配和元素复制的开销，从而提高性能。
3. `reserve` 通常用于已知将要向容器中插入大量元素的情况。

### resize

`v.resize(length,element)`重新指定容器的长度为$length$，若容器变长则以默认值（element）填充新位置，如果容器变短则末尾超出的元素被删除

### assign

```c++
    vector<int> v1;
    for (int i = 0; i < 10; i++) {
        v1.push_back(i);
    }
    vector<int> v2;
    v2.assign(v1.begin(), v1.end()); //提供两个迭代器，两个迭代器区间中的元素都赋值给vector容器，区间为前闭后开
    vector<int> v3;
    v3.assign(10,100);//把10个100赋给v3
```

### iterator

#### 定义

`容器类名::iterator 迭代器名`

```c++
vector<int> a{1, 2, 3, 4, 5};
vector<int>::iterator it;
it = a.begin()+2;
cout << *it << endl;
```

$begin()$：返回指向容器中第一个元素的正向迭代器

$end()$：返回指向容器**最后一个元素的后一个位置**的正向迭代器

$rbegin()$：返回指向最后一个元素的反向迭代器

$rend()$：返回指向**第一个元素的前一个位置**的反向迭代器

**除了set和string以外的stl容器都不支持*(it+i)的访问方式**

### resize

vector是一个动态数组，允许在运行时改变其大小

##### 语法

```C++
void resize(size_t count);
void resize(size_t count, const T& value);
```

```c++
std::vector<int> vec = {1, 2, 3};
vec.resize(5); // vec 变为 {1, 2, 3, 0, 0}
vec.resize(7, 9); // vec 变为 {1, 2, 3, 0, 0, 9, 9}
```

如果新的大小大于当前大小，则会增加vector的大小，小于当前大小，则会移除多余元素

### assign

##### 1.初始化容器

```c++
 
// 初始化vector容器为10个0
vector vec(10);
vec.assign(10, 0); // 初始化为10个0
```

##### 2.复制容器元素

```c++
 
vector vec1 = {1, 2, 3, 4, 5};
vector vec2;
vec2.assign(vec1.begin() + 2, vec1.end()); // 复制vec1的3~5元素到vec2
```

##### 3.插入元素

```c++
 
vector vec = {1, 2, 3, 4, 5};
vec.assign({6, 7, 8}); // 插入3个元素，等价于vec={6, 7, 8}
```

##### 4.删除元素

```c++
 
deque deq = {1, 2, 3, 4, 5};
deq.assign(deq.begin(), deq.end() - 2); // 删除末尾2个元素
```

### 添加一行

```c++
//插入一行数组：将in_row数组插入到第2行！ 
vector<int> in_row(5,6);//初始化一个数组，包含5个元素并且全为6
a.insert(a.begin()+2,in_row);
```

### 添加一列

```c++
for(int i=0;i<a.size();i++)
{
	a[i].insert(a[i].begin()+2,9);
}
```

### 删除一行

```c++
a.erase(a.begin() + 2, a.begin() + 3);
```

### 删除一列

```c++
//删除a的第二列
for (int i = 0; i < a.size(); i++)
{
    a[i].erase(a[i].begin() + 2, a[i].begin() + 3);
}
```

