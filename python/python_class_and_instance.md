---
layout: post 
---

## 引言：

我们常写的`python`代码中如果给一个对象增加一个属性，比如给一个`Student`类增加一个`name`属性，那么可以直接在`__init__(self, name)`增加一个`self.name = name`即可。这样每新建一个对象，就会有一个对应的`name`属性与之对应，且对象之间的`name`是不共享的。如下：

```python
class Student(object):
    def __init__(self, name):
        self.name = name
```

然而有时候我们希望**让所有新建出来的对象共享同样的一个属性**，这时候就需要用到`类属性`的概念了。在面向对象的语言中，我们总是说一切皆对象，因此可以把我们新定义的类看做一个类对象，这样给类添加的属性叫做类属性。其表现形式为如下：

```python
class Student(object):
    # 属性
    num = 0                     # 类属性
    # 方法
    def __init__(self, name):
        self.name = name        # 实例属性
```

举个类比的例子，**`类属性`的作用有点像`全局变量`**：我们为了让全局范围内都能够访问一个变量，可以定义一个全局变量；而为了让所有的对象都能够访问同一个变量，就可以定义一个类属性来满足需求。

<!-- more -->

## 新建一个对象的机制

当一个类定义完成后，一但创建一个该类的实例对象，该实例保存了两样东西：
1. 实例属性，也就是我们通过`__init()__`方法给它的一些属性(通常我们都是这么做的)。在对象之间不可以共享。
2. 一个特殊的属性，知道该实例是通过那个类创建的。（这样做的好处是没有保存该类的各种方法，而是只在类中定义了一份，以节省空间）

从上面的分析来看，一个类为了保存各种方法，因此也是要内存空间的，因此一个**`类对象`**也可以包含两样东西：
1. 类属性，可以在对象之间共享。
2. 各种方法

## 一个例子

### 类属性归类所有

还是以`Student`为例，我们有如下需求：

> 每新实例化一个`Student`类都有一个计数器来记录当前已经创建了多少个对象了.

因此可以使用类属性做到这个需求,代码如下：
```python
class Student(object):

    num = 0

    def __init__(self, name):
        self.name = name
        Student.num += 1        # 注意这里使用的不是self.num,而是Student.num

    def get_num(self, ):
        return Student.num      # 注意这里使用的不是self.num,而是Student.num 

s1 = Student('xiaoming')
print(s1.get_num())

s2 = Student('xiaohong')
print(s2.get_num())

s3 = Student('huluwa')
print(s3.get_num())

#---
# Output
1
2
3
```
从输出的结果可以看出，所有的对象都是共享类属性的。

### 类属性可以通过实例对象调用

当我们定义了一个类属性后，这个属性虽然归类所有，但类的所有实例都可以访问到。如下：
```python
class Student(object):

    num = 0

    def __init__(self, name):
        self.name = name
        Student.num += 1        # 注意这里使用的不是self.num,而是Student.num

    def get_num(self, ):
        return Student.num      # 注意这里使用的不是self.num,而是Student.num 

s1 = Student('xiaoming')
print(s1.num)                   # 这里直接通过对象访问类属性依然可以访问到

s2 = Student('xiaohong')
print(s2.num)

s3 = Student('huluwa')
print(s3.num)

#---
# Output
1
2
3
```
***注意：*** 
> 类属性和实例属性尽量不要用同样的名字，否则通过对象调用类属性时，会被相同名字的实例属性覆盖。详情见[1].

## 总结

综上，我们可以总结实例属性和类属性有如下特点：

- 实例属性：
    和具体的某个实力对象有关系，并且一个实例对象和另一个实例对象是不共享属性的。

- 类属性：
    类属性所属于类对象，并且多个实例对象之间共享同一个类属性。

## 参考

[\[1\]. 廖雪峰python](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
