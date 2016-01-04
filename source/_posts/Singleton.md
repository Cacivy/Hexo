---
title: Singleton
tags:
  - Design Patterns
  - C#
categories: Code
date: 2015-01-16
---

### 单例模式

[Wiki](http://zh.wikipedia.org/wiki/%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F)

> 单例模式，也叫单子模式，是一种常用的软件设计模式。在应用这个模式时，单例对象的类必须保证只有一个实例存在。

### 要点
1. 某个类只能有一个实例
2. 必须自行创建这个实例
3. 必须自行向整个系统提供这个实例

<!-- more -->

#### 构建方式

  + 懒汉方式。指全局的单例实例在第一次被使用时构建。

``` csharp
public class Singleton
{
    //volatile:不被线程访问和修改
    private static volatile Singleton Instance = null;
    private static object _lock = new object();
    //私有构造
    private Singleton() { }
    //锁定对象只能被一个线程访问并返回
    public static Singleton getInstance()
    {
        if (Instance == null)
        {
            lock (_lock)
            {
                if (Instance == null)
                {
                    Instance=new Singleton();
                }
            }
        }
        return Instance;
    }
}
```

> 上述代码使用了双重锁方式较好地解决了多线程下的单例模式实现。先看内层的if语句块，使用这个语句块时，先进行加锁操作，保证只有一个线程可以访问该语句块，进而保证只创建了一个实例。再看外层的if语句块，这使得每个线程欲获取实例时不必每次都得加锁，因为只有实例为空时（即需要创建一个实例），才需加锁创建，若果已存在一个实例，就直接返回该实例，节省了性能开销。

  + 饿汉方式。指全局的单例实例在类装载时构建。

``` csharp
public class Singleton
{
    //实例私有只读对象
    private static readonly Singleton Instance = new Singleton();
    //私有构造
    private Singleton(){}
    //返回对象
    public static Singleton GetInstance()
    {
        return Instance;
    }
}  
```

> 上面使用的readonly关键可以跟static一起使用，用于指定该常量是类别级的，它的初始化交由静态构造函数实现，并可以在运行时编译。在这种模式下，无需自己解决线程安全性问题，CLR会给我们解决。由此可以看到这个类被加载时，会自动实例化这个类，而不用在第一次调用GetInstance()后才实例化出唯一的单例对象。


