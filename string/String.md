# string

### 声明

```c++
std::string a;
string str1;               //生成空字符串
string str2("123456789");  //生成"1234456789"的复制品
string str3("12345", 0, 3);//结果为"123"
string str4("0123456", 5);  //结果为"01234"
string str5(5, '1');       //结果为"11111"
string str6(str2, 2);      //结果为"3456789"

```

### 获取长度

```c++
a.size();
a.length();
strlen(a.c_str());
```

### find

`find(str,pos)`函数可以用来查找字符串在`pos`(含)之后一个字符/字符串第一次出现的位置（`pos`默认是0），如果没有出现返回`string::npos`（被定义为$-1$）

> 返回的位置也是从0开始

```c++
string a="hello world";
    int pos=a.find("lo");
	if(a.find("lo")==string::npos){
        cout<<"Not found";
}
    cout<<pos;
//pos=3
```

`rfind(str,pos)`可以实现从字符串后面往前查找第一个字符串出现的位置

### substr

`substr（pos，len）`函数的参数返回从`pos`位置开始计数（包含pos）截取最多`len`个字符组成的字符串（如果从`pos`开始的后缀长度不够`len`个，就截取这个后缀），如果没给定len的值，默认从pos到结尾

```c++
string a="hello world";
    cout<<a.substr(6,6);
//world
```

### insert

`insert（index,count,ch）`和`insert（index,str）`，分别表示在`index`处连续插入`count`次字符串`ch`和插入一次字符串`str`

- `push_back`在尾部插入一个字符，尾插也可以直接`+="string"`

### erase

- `erase（index,count）`函数将字符串`index`位置开始（含）的`count`个字符删除（若不传参给`count`，则表示删除`index`位置及以后的所有字符）
- `erase(first,end)`删除区间`[first,end)`上所有的字符

> 左闭右开

### replace

`std::replace（pos,count,str）`和`std::replace（first,last,str）`，前者表示从pos位置开始count个字符的字串替换为str，后者表示从`first`开始（含）、`last`结束（不含）的子串替换为`str`，其中`first`和`last`均为**迭代器**（指针）

### count

count（起始位置，结束位置，要查询的**单个字符**）

```c++
string s="YYYYY";
cout<<count(s.begin(),s.end(),'Y');
```

### tolower toupper

`tolower`是大写转小写

```c++
void solve() {
    string a="AAA";
    for(int i=0;i<a.size();i++) {
        a[i]=tolower(a[i]);
    }
    cout<<a;
}
```

`toupper`是小写转大写

```c++
void solve() {
    string a="aaaCc";
    for(int i=0;i<a.size();i++) {
        a[i]=toupper(a[i]);
    }
    cout<<a;
}
```

通过STL的`transform`算法配合tolower和toupper来实现

```c++
void solve() {
    string a="aaaCc";
    transform(a.begin(),a.end(),a.begin(),::tolower);
    cout<<a;
}
```

### string的排序

`sort(a.begin(),a.end())`

默认按照**ASCLL码**升序排列

```c++
void solve() {
    string a="aCaCc";
    sort(a.begin(),a.end());
    cout<<a;// a=CCaac
}
```

可自定义排序函数

```c++
bool cmp(char a,char b) {
    return a>b;
}
void solve() {
    string a="aCaCc";
    sort(a.begin(),a.end(),cmp);
    cout<<a;// a=caaCC
}
```

### islower isupper isalpha isdigit isalnum

头文件`ctype.h`

函数返回值**bool**

```c++
isalpha(ch)// 判断是否为字母
isdigit(ch)// 判断是否为数字
islower(ch)// 判断是否为字母小写
isupper(ch)// 判断是否为字母大写
isalnum(ch)// 判断是否为英文或数字
```

### 读入带空格的字符串

```c++
std::string s;
std::getline(std::cin,s);
```

如果前面有其他输入，需要加上`std::cin.ignore`

### to_string

头文件`#include<cstring>`

```c++
std::string s=std:;to_string(int x)
```

将x转换为字符串类型，x可以是double,long long ,long int

### stoi

将n进制的字符串转换为十进制

```
stoi(字符串，起始位置，n进制（默认10进制）)
```

头文件`#include<cstring>`

```c++
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
	string str = "100";
	int x = stoi(str, 0, 2); //将二进制"100"转化为十进制x
	cout << x << endl;
	return 0;
}

```

**注：** stoi()函数如果传入的字符串s中含有不是数字的字符，则只会识别到从开头到第一个非法字符之 前，如果第一个字符就是非法字符则会报错

还有stod，stol，stoul，stoull，stoull，

### reverse

`std::reverse(begin,end)`

区间也是**左闭右开**

### sstream

`<sstream>`库定义了三种类：`istringstream`、`ostringstream`和`stringstream`，分别用来进行流的输入、输出和输入输出操作

#### istringstring

```c++
#include<iostream>
#include<sstream>

int main () {
    std::string s;
    std::getline(std::cin, s);

    std::istringstream is(s);
    std::string tok;
    int sum = 0;
    while (is >> tok) {
        // std::cout << tok;
        sum += stoi(tok);
    }
    std::cout << sum;
}
```

#### stringstream

```c++
#include<iostream>
#include<sstream>

int main () {
    std::string s;
    std::stringstream ss;
    int n, i, sum, a;
    std::cin >> n;
    getline(std::cin, s); // 讀取換行
    for (i=0; i<n; i++)
    {
        getline(std::cin, s);
        ss.clear();
        ss.str(s);
        sum=0;
        while (1)
        {
            ss >> a;
            if (ss.fail()) break;
            sum+=a;
        }
        std::cout << sum << "\n";
    }

                        
}
```

int转string

```c++
#include <string>
#include <sstream>
#include <iostream> 

int main()
{
    std::stringstream stream;
    std::string result;
    int i = 1000;
    stream << i; //将int输入流
    stream >> result; //从stream中抽取前面插入的int值
    std::cout << result << std::endl; // print the string "1000"
} 

```

### transform

将某操作应用于指定范围的每个元素

格式：`transfrom(begin,end,result,option)`

```c++
transform(a.begin(),a.end(),a.begin(),::tolower);//小写
```

