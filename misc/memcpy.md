# memcpy

### 什么类型都可以拷贝

```c++
#include <stdio.h>
#include <string.h>
int main()
{
	int arr1[] = { 1,1,1,1,1,1,1,1 };
	int arr2[] = { 5,5,5,5,5 };
	memcpy(arr1, arr2, 20);
	return 0;
}
```

#### 把arr2中的前20个字节的内容拷贝到arr1中

# strcpy

#### 用法同上面一样，但只能拷贝字符类型

# memset

### 头文件 cstring

# sizeof

### 计算变量再内存中占空间的大小，单位是字节