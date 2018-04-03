---
layout: post
title: 几个C++关键字的用法总结(Updating)
date: 2018-04-03
tags: C++
---

### 作用域运算符::

　作用域运算符::是运算符中等级最高的，可以分成三种：

- **全局作用域符：**

　当全局变量在某个局部作用中与其中某个局部变量重名，那么就可以用全局作用域符::来区分
	
	```
	int x;   // 全局变量 
	void func()
	{ 
	　int x;   // 局部变量 
	　x = x + x;   // 局部变量，代码块中局部变量会覆盖全局变量 
	　::x = ::x + x;   // 前两个x为全局变量，最后的x为局部变量 
	}
	```

- **类作用域符：**

　为了避免不同的类有名称相同的成员，可以用类作用域符进行区分，类作用域符前面是类的名称，后面是该类的成员名称

	```
	class A
	{
	public:
	　void func();
	}
	
	class B
	{
	public:
	　void func();
	}
	
	void A::func()
	{
	　// ...
	}
	
	void B::func()
	{
	　// ...
	}
	```

- **命名空间作用域符：**

　命名空间作用域符可以区分不同命名空间中的成员，命名空间作用域符前面是命名空间的名称，后面是该命名空间的成员名称

	```
	#include<iostream>
	using namespace std;
	
	namespace A
	{
	　int x = 1;
	　
	　void fun()
	　{
	　　cout << "A" << endl;
	　}
	}
	
	namespace B
	{
	　int x = 2;
	　
	　void fun()
	　{
	　　cout << "B" << endl;
	　}
	　
	　void fun2()
	　{
	　　cout << "BB" << endl;
	　}
	}
	
	int main()
	{
	　cout << A::x << endl;
	　cout << B::x << endl;
	
	　using namespace B;
	
	　A::fun();
	　fun();
	　fun2();
	　
	　return 0;
	}
	```



