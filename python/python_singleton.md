---
layout: post 
---

## 简介：

本文主要讲两个问题：

1. pyhton创建一个对象的过程。

2. 单例设计模式的一种实现方式。

## python 创建一个对象的过程

当我们实例化一个对象的时候，基本上可以分为如下步骤：

1. 调用`__new__(cls)`方法来创建一个对象，然后找了一个变量来接受`__new__`的返回值，这个返回值表示创建出来的对象的引用

2. 调用`__init__(刚创建出来的对象的引用)`方法，初始化成员变量。

3. 返回对象的引用

注意，这里`__new__`方法里面需要传递的参数是`cls`，指的是


>> 我们可以拿`python`创建一个对象的过程和`c++`的构造函数做个对比。显然`c++`的构造函数即负责创建对象，又负责初始化成员变量； 而`python`是通过两个步骤来完成的：`__new__(cls)`方法只负责创建对象，`__init__`方法只负责初始化成员变量。

>> 从上面的总结来看，一个常见的误区就是**错误的把`__init__`方法等价于构造函数，严格来说他们是不等价的。**

## 单例模式的一种实现方式

有了上述`__new__`方法的机制，我们可以设计一个单例模式如下：

这里增加了一个附加的东西，即**设计一个单例的同时只初始化一次成员变量**。

```python
class Singleton(object):
    __instance = None
    __init_flag = False 

    def __new__(cls, *args, **kwargs):
        if cls.__instance == None:
            cls.__instance = object.__new__(cls)
        return cls.__instance

    def __init__(self, num): # 这里通过设置一个类属性的方式实现了只初始化一次成员变量的目的
        if not Singleton.__init_flag:
            self.num = num
            Singleton.__init_flag = True

s = Singleton(100)
print(s.num)

s1 = Singleton(200) # 第二次初始化参数时不会打印200
print(s1.num)

# Output
100
100
```
