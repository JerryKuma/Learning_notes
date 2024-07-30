# 字符串

## 基本概念

在c++中，字符串是一种字符数组，但是经过封装和改造，作为标准库中的一种数据结构，被广泛使用。其自带一些处理函数，简化了字符串相关的操作。

## 基本函数

### 输入字符串

假如想要在一行内输入多个字符串，则直接使用cin即可。  

如果想要将一行内容全部存储为一个字符串，则需要使用getline或者cin.getline函数，具体用法如下：  

```
#include<bits/stdc++.h>
using namespace std;

对于字符数组,用cin.getlinne输入
int main()
{
	char str[25] = { 0 };
	cin.getline(str, 25);
	printf("%s\n", str);
	return 0;
}

对于字符串,使用ggetline输入
int main(){
	string cmp;
	string title;
	getline(cin,title);
	cout<<title;
	return 0;
}
```

### 查找子串

string 类有一些查找子串和字符的成员函数，它们的返回值都是子串或字符在 string 对象字符串中的位置（即下标）。如果查不到，则返回 string::npos。string: :npos 是在 string 类中定义的一个静态常量。这些函数如下：

* find:从前往后查找子串或字符出现的位置,如果找不到,会返回 string::npos

```
    if (b.find(a)==string::npos){
        cout<<-1<<endl;
    }
```

* rfind：从后往前查找子串或字符出现的位置。
* find_first_of：从前往后查找何处出现另一个字符串中包含的字符。例如：s1.find_first_of("abc");  //查找s1中第一次
* 现"abc"中任一字符的位置
* find_last_of：从后往前查找何处出现另一个字符串中包含的字符。
* find_first_not_of：从前往后查找何处出现另一个字符串中没有包含的字符。
* find_last_not_of：从后往前查找何处出现另一个字符串中没有包含的字符。
* string.substr（size\_t pos，size\_t len):从str中拷贝一部分形成新字符串,pos为起始位置,len为子串长度 省略len变量输入 可以从pos位置开始到末尾 用于提取字符串后缀
* tolower:把大写字符转小写字符
