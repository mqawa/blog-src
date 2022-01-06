---
id: 21122802
title: C++ I/O 流的结构图
date: 2021-12-28 10:22:06
description: C++ I/O 流的基本结构图，包括 iostream fstream sstream
tags: cpp
categories: 
  - [学习笔记, C++]
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/cpp/inc/cpp.jpg
---
***
{% mermaid %}
graph BT

ios_base(ios_base)-->_Iosb(_Iosb)

basic_ios(basic_ios)-->ios_base(ios_base)
basic_istream(basic_istream)-->basic_ios(basic_ios)
basic_ostream(basic_ostream)-->basic_ios(basic_ios)

basic_ifstream(basic_ifstream)-->basic_istream(basic_istream)
basic_istringstream(basic_istringstream)-->basic_istream(basic_istream)
basic_iostream(basic_iostream)-->basic_istream(basic_istream)

basic_ofstream(basic_ofstream)-->basic_ostream(basic_ostream)
basic_ostringstream(basic_ostringstream)-->basic_ostream(basic_ostream)
basic_iostream(basic_iostream)-->basic_ostream(basic_ostream)

basic_fstream(basic_fstream)-->basic_iostream(basic_iostream)
basic_stringstream(basic_stringstream)-->basic_iostream(basic_iostream)

{% endmermaid %}
***