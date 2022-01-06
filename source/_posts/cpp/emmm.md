---
id: 21120602
title: 一些乱七八糟的问题
date: 2021-12-06 14:36:00
description: 一些小代码，没有独立写文的必要，整合在这篇里
tags: cpp
categories: 
  - [学习笔记, C++]
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/cpp.jpg
---

***

## 简介

**一些闲来无事写的小代码，没有独立写文的必要，就都放在这里了**

***

## 重载 to_string

{% folding, 重载to_string使得map，vector等容器可以转化为string字符串 %}

**使用 ostringstream 进行链式字符串拼接**

```c++
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
#include <map>

using namespace std;

string to_string(string str) {
	return str;
}

template<typename K, typename V>
string to_string(const pair<K, V>& p) {
	ostringstream o;
	o << to_string(p.first) << ":" << to_string(p.second);
	return o.str();
}

template<typename T>
string to_string(const T& begin, const T& end) {
	ostringstream o;
	for (T it = begin; it != end; ++it) {
		if (it != begin)
			o << ", ";
		o << to_string(*it);
	}
	return o.str();
}

template<typename K, typename V>
string to_string(const map<K, V>& m) {
	ostringstream o;
	o << "{ " << to_string(m.begin(), m.end()) << " }";
	return o.str();
}

template<typename T>
string to_string(const vector<T>& v) {
	ostringstream o;
	o << "[ " << to_string(v.begin(), v.end()) << " ]";
	return o.str();
}

int main() {
	map<string, string> m;
	m.insert(pair<string, string>("name", "test"));
	m.insert(pair<string, string>("age", "22"));
	m.insert(pair<string, string>("sex", "男"));
	vector<int> v = { 1,2,3,4,5 };
	cout << to_string(m) << endl;
	cout << to_string(v) << endl;
	return 0;
}
```

{%  endfolding %}

***

## cout 输出true, false

{%  folding, 使cout输出true，false而不是1，0 %}

**在默认情况下，cout会将true, false转换为1, 0输出，而不是输出true, false**

**解决： 使用boolalpha，头文件iostream**

**当使用boolalpha后，以后的bool类型结果都将以true或false形式输出，除非使用 noboolalpha取消 boolalpha流的格式标志**

```c++
#include<iostream>

using namespace std;

int main() {
	cout << "boolalpha" << endl;
	cout << boolalpha;
	cout << "true : " << true << endl;
	cout << "false : " << false << endl << endl;
	cout << "noboolalpha" << endl;
	cout << noboolalpha;
	cout << "true : " << true << endl;
	cout << "false : " << false << endl;
	return 0;
}
```



{%  endfolding %}

***

## 函数指针

{%  folding, 函数指针的使用 %}

**函数指针的定义：函数返回值类型 (\* 指针变量名) (函数参数列表);**

```c++
void func(int num){}
void (*p)(int);//定义了一个返回值为void，参数列表为int，且只有一个参数的函数指针
p = func;//指向函数
```

**函数指针的使用**

```c++
#include<iostream>

using namespace std;

void func(int num) {
	cout << num << endl;
}

int main() {
	void(*p)(int) = func;
	p(1);//理解：函数名就是指针，所以使用函数指针与使用函数一样
	(*p)(1);//理解：指针p指向函数，所以要解析指针
	return 0;
}
```

{%  endfolding %}

***

## 构造和析构函数是否可以为虚函数？

{%  folding, 构造和析构函数是否可以为虚函数？ %}

**构造函数不可以为虚函数，析构函数可以**

{%  folding, 为什么构造函数不可以为虚函数 %}

**虚函数的调用需要虚函数表指针，而该指针存放在对象的内容空间中；若构造函数声明为虚函数，那么由于对象还未创建，还没有内存空间，更没有虚函数表地址用来调用虚函数——构造函数了。**

{%  endfolding %}

{%  folding, 为什么析构函数可以为虚函数 %}

**当要使用基类指针或引用调用子类时，最好将基类的析构函数声明为虚函数，否则可能存在内存泄露的问题。**

```c++
#include <iostream>

using namespace std;

class Base {
public:
	Base() {
		cout << "make Base" << endl;
	}

	~Base() {
		cout << "del Base" << endl;
	}
};
class Son : public Base {
public:
	Son() {
		cout << "make Son" << endl;
	}

	~Son() {
		cout << "del Son" << endl;
	}
};

int main() {
	Base* p = new Son();
	delete p;
}
```

![构造01](../../res/images/cpp/emmm/构造01.png)

**根据输出可见，Son的析构函数未被调用，存在内存泄露问题，将Base类的析构函数声明为虚函数即可解决**

```c++
#include <iostream>

using namespace std;

class Base {
public:
	Base() {
		cout << "make Base" << endl;
	}

	virtual ~Base() {
		cout << "del Base" << endl;
	}
};
class Son : public Base {
public:
	Son() {
		cout << "make Son" << endl;
	}

	~Son() {
		cout << "del Son" << endl;
	}
};

int main() {
	Base* p = new Son();
	delete p;
}
```

![构造01](../../res/images/cpp/emmm/构造02.png)

{%  endfolding %}

{%  endfolding %}

***

## 成员变量初始化顺序

{%  folding, 成员变量初始化顺序 %}

**成员变量的声明顺序，决定了成员变量的初始化顺序。假设 Date 类中的构造函数为：**

```c++
public:
	Date() : y(2016), m(2), d(4) {}
```

**此时，成员变量，在类中的声明顺序 = 构造函数初始化列表顺序，故 y, m, d 都能被顺利的 初始化为对应的值。**

**而当成员变量，在类中的声明顺序 ≠ 构造函数初始化列表顺序 时，如下：**

```c++
public:
	Date() : y(2016), d(4), m(d-2) {}
```

**根据成员变量的声明顺序，y 首先被初始化为 2016，然后再初始化 m，但由于 d 并未被初始 化，所以 m 的值是随机的，最后初始化 d 为 4**

**因为，{% span red, 类的成员变量在初始化时，其初始化的顺序只与声明顺序有关，而与初始化顺序无关。 %}**

```c++
#include <iostream>

using namespace std;

class Date
{
public:
	Date() :y(2021), d(4), m(d - 2) {};

	void printDate() {
		cout << "y : " << y << endl;
		cout << "m : " << m << endl;
		cout << "d : " << d << endl;
	}
private:
	int y, m, d;
};

int main() {
	Date date;
	date.printDate();
	return 0;
}
```

![初始化](../../res/images/cpp/emmm/初始化.png)

{%  endfolding %}

***

## C++的函数重载

{%  folding, C++的函数重载 %}

+ **为什么要进行函数重载**
  + **为了简化编程**
  + **提高编程效率**
  + **增加代码可读性**

+ **函数重载的规则**
  + **函数名相同**
  + **函数的参数列表不同（参数个数，类型，顺序不同等）**
  + **函数的返回值可以不同**
  + **仅仅返回类型不同不足以成为函数的重载**

+ **命名倾轧**
  + **C++函数重载底层实现原理是C++利用name mangling(倾轧)技术，来改名函数 名，区分参数不同的同名函数。**
  + **编译器通过函数名和其参数类型识别重载函数。为了保证类型安全的连接（typesafe linkage），编译器用参数个数和参数类型对每一个函数标识符进行专门编 码，这个过程有时称为“名字改编”（name mangling）或“名字修饰”（name decoration）。类型安全的连接使得程序能够调用合适的重载函数并保证了参数 传递的一致性。编译器能够检测到并报告连接错误。**

+ **查看符号表**

  ```c++
  #include<iostream>
  
  int func(int a) {
  	return a;
  }
  double func(double a) {
  	return a;
  }
  
  int main() {
  	return 0;
  }
  ```

  + **使用 g++ -c 只编译不链接，生成目标文件**

  + **使用 nm 查看目标文件（.o）**

    ![重载](../../res/images/cpp/emmm/重载.png)

{%  endfolding %}

***