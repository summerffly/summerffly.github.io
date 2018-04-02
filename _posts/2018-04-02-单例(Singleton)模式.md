---
layout: post
title: 单例(Singleton)模式
date: 2018-04-02
tags: 设计模式
---

　对于具有一定开发经验的程序员，几乎都在项目中使用或者接触过单例模式，单例模式是软件设计模式中最常用的模式之一

　在计算机系统中，线程池、缓存、日志对象、打印机、显卡的驱动程序对象等常被设计成单例实现，这些应用都或多或少具有资源管理器的功能，比如每台计算机一定只能有一个Printer Spooler，以避免两个打印作业同时输出到打印机中，选择单例模式就是为了避免不一致状态，避免发生冲突

### 单例模式的定义

　单例模式涉及到一个单一的类(单例类)，该类负责创建自己的对象，同时确保只有单个对象被创建，这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象，可以概括为以下三点：

- 单例类只能有一个实例

- 单例类必须自己创建自己的唯一实例

- 单例类必须给所有其他对象提供这一实例的访问方式

![](/images/posts/DesignPattern/Singleton.jpg)

### 单例模式的代码实现(C++)

　单例模式有两种实现方式 —— 懒汉方式和饿汉方式：　

- 懒汉方式：在第一次用到类实例的时候才会去实例化，访问线程比较少时，可使用懒汉方式，用时间换空间

- 饿汉方式：单例类定义的时候就进行实例化，访问线程比较多时，可使用饿汉方式，用空间换时间

```
// 单例模式的懒汉方式实现
// 为了保证线程安全，需要加锁

class Singleton
{
protected:
　Singleton();
public:
　static pthread_mutex_t mutex;
　static Singleton* GetInstance();
private:
　static Singleton* p;
}

pthread_mutex_t Singleton::mutex;
Singleton* Singleton::p = NULL;

Singleton::Singleton()
{
　pthread_mutex_init(&mutex);
}

Singleton* Singleton::GetInstance()
{
　if( p == NULL)
　{
　　pthread_mutex_lock(&mutex);
　　p = new Singleton();
　　pthread_mutex_lock(&mutex);
　}
　
　return p;
}

```

```
// 单例模式的饿汉方式实现
// 饿汉方式本身就是线程安全的

class Singleton
{
protected:
　Singleton() {}
public:
　static Singleton* GetInstance();
private:
　static Singleton* p;
}

Singleton* Singleton::p = Singleton();

Singleton* Singleton::GetInstance()
{
　return p;
}

```



