---
id: 21120101
title: 前置自增（减）和后置自增（减）的区别以 及效率
date: 2021-12-01 10:45:06
description: 本文对 C++ 的前置自增（减）和后置自增（减）的区别和效率进行分析，包括内置数据类型和自定义数据类型
tags: cpp
categories: 
  - [学习笔记, C++]
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/cpp.jpg
---

***

### **两者的区别**

- **《C专家编程》中说道：“ ++x 表示取 x 的地址，增加它的内容，然后把值放到寄存器中，x++ 则表示取 x 的地址，把他的值装入寄存器中，然后增加内存中的 x 的值”。**
- **前置自增（减）返回的是对象的引用，后置自增（减）返回的是对象的拷贝，这就意味着对前置自增（减）的操作会操作对象本身，而后置自增（减）不会。**
- **在现代 IDE 中，会优化内置数据类型，统一为前置自增（减），来优化性能，但是对于自定义数据类型的重载，前置自增（减）效率优于后置自增（减）。**

***

### **为什么前置自增（减）优于后置自增（减）**

- **对于内置数据类型，我们查看反汇编**

  ```c++
    #include <iostream>
    
    using namespace std;
    
    int main(){
        int i = 0;
        ++i;
        i++;
        return 0;
    }
  ```

  + ![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/int.png)

  + **我们发现，运算流程相同，说明编译器对后置自增（减）运算进行了优化**

- **对于自定义数据类型，我们重载自增（减）运算符后查看反汇编**

  ```c++
    #include <iostream>
    
    using namespace std;
    
    class Test {
    public:
    	Test() :num(0) {};
    	Test operator++(int) {//后置自增
    		this->num++;
    		return *this;
    	}
    	Test& operator++() {//前置自增
    		this->num++;
    		return *this;
    	}
    	int num;
    };
    int main() {
    	Test t;
    	++t;
    	t++;
    	return 0;
    }
  ```

  + ![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/Test.png)

- **或者可以通关禁用拷贝构造函数的方式进行验证**

  ```c++
    #include <iostream>
    
    using namespace std;
    
    class Test {
    public:
    	Test() :num(0) {};
    
    	Test operator++(int) {
    		this->num++;
    		return *this;
    	}
    
    	Test& operator++() {
    		this->num++;
    		return *this;
    	}
    
    	int num;
    private:
    	Test(const Test& test);//禁用拷贝构造函数
    	Test& operator=(const Test& test);//禁用赋值运算符
    };
    int main() {
    	Test t;
    	++t;
    	t++;
    	return 0;
    }
  ```

  + ![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/error.png)

  + **会发现后置自增（减）链接错误，说明后置自增（减）调用了拷贝构造函数**

- **总结：不考虑返回值的话，前置自增（减）在编译器不优化的情况下，效率会优 于后置自增（减），因为后置自增（减）会对数据进行一次拷贝，如果数据为自定义数据类型或者为迭代器等，拷贝的代价会非常大，当前置和后置的功能一样时，优先使用前置自增（减）。**

***