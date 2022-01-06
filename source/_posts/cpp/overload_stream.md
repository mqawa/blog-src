---
id: 21120601
title: 重载流操作符时遇到的问题（输出自定义数据类型到文件）
date: 2021-12-06 10:26:00
description: 为了使自定义数据类型可以使用流操作符输出到文件，重载了流操作符，因此引起了一系列错误
tags: cpp
categories: 
  - [学习笔记, C++]
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/cpp.jpg
---

***

### 如下代码，将Test类通过重载流操作符输出到文件，编译时会报错（二义性）



```c++
#include <iostream>
#include <fstream>

using namespace std;

class Test
{
public:
	Test(int num) {
		this->num = num;
	}
	int num;
};

ofstream& operator<<(ofstream& ofs, const Test& test) {
	ofs << test.num;
	return ofs;
}

void write() {
	ofstream ofs("test.txt", ios::out | ios::trunc);
	ofs << Test(10) << endl;
	ofs.close();
}

int main() {
	write();
	return 0;
}

```

![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/overload_stream/error.png)

***

### 错误原因

+ **std内置函数**

  ```c++
  std::ostream& operator<<(std::ostream&, const int);
  ```

+ **自定义函数**

  ```c++
  ofstream& operator<<(ofstream& ofs, const Test& test);
  ```

+ **当运行到**

  ```c++
  ofs << test.num;
  ```

+ **<< 运算符的两个参数类型分别为（ofstream，int）**

+ **对于内置函数，第一个参数为父类引用指向子类对象（ostream -> ofstream），第二个参数完全匹配。**

+ **对于自定义函数，第一个参数完全匹配，第二个参数因为Test类的构造函数可以隐式调用。Test test = 1; 是合法的**

+ **编译器不知道调用哪个函数来处理这个语句**

***

### 解决办法

+ **将构造函数前加explicit关键字，使构造函数不可进行隐式转换**

  ```c++
  class Test
  {
  public:
  	explicit Test(int num) {
  		this->num = num;
  	}
  	int num;
  };
  ```

+ **将 << 的参数改为 ostream，重载ostream即可**

  ```c++
  ostream& operator<<(ostream& ofs, const Test& test) {
  	ofs << test.num;
  	return ofs;
  }
  ```
***