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
	#include <iostream>
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

### 关键字extern

　C++支持分离式编译机制，一个完整的程序或项目可以分割为若干个.cpp源文件，每个.cpp源文件单独编译生成.obj目标文件，最后将所有.obj目标文件链接成一个单一的可执行文件，如果一个.cpp源文件要使用另一个.cpp源文件定义的变量，应该如何调用？ —— C++中解决方法是将 `变量声明` 与 `变量定义` 分离：

　`变量声明` 规定了变量的类型和名字，但仅仅只是告诉编译器，有个某类型的变量会被使用，但是编译器并不会为它分配任何内存，使得名字为本程序文件所知道，比如一个文件如果想使用在另外一个文件中定义的变量，则必须包含对那个变量名字的声明，关键字extern就被用来实现 `变量声明`

　`变量定义` 同样规定了变量的类型和名字，不同于 `变量声明` 的是，`变量定义` 还会给变量申请存储空间，还可能给变量赋予一个初始值，如果多个文件中都定义了同一个变量，编译时会产生变量重定义冲突错误

	```
	extern int i;   // 变量声明
	int j;   // 变量定义
	
	// 需要注意，任何一个显式初始化的声明都将成为定义，而不管有没有extern
	extern double pi = 3.1415926;
	```

　关键字extern主要有以下三种使用方法：

- **引用同一个文件中的变量**

```
#include <stdio.h>

int func();

int main()
{
　func();
　extern int num;   // 引用同一个文件中的变量
　printf("%d", num);
　
　return 0;
}

int num = 3;

int func()
{
　printf("%d\n",num);
}
```

- **引用另一个文件中的变量**

```
// main.cpp

#include <stdio.h>

int main()
{
　extern int num;   // 引用另一个文件中的变量
　printf("%d",num);
　
　return 0;
}
```

```
// branch.cpp

#include <stdio.h>

int num = 5;
```

- **引用另一个文件中的函数**

```
// main.cpp

#include <stdio.h>

int main()
{
　extern void func();   // 引用另一个文件中的函数
　func();
　
　return 0;
}
```

```
// branch.cpp

#include <stdio.h>

void func()
{
　printf("func");
}
```

