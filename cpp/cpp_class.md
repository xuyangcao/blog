---
layout: post
---

# 深拷贝和浅拷贝

- 浅拷贝：简单的赋值拷贝操作。
- 深拷贝：在堆内存从新申请空间，进行拷贝操作。

注意这里和python中的深拷贝浅拷贝有区别。


# 类&对象

## 类访问权限修饰符

**piblic：** 成员在类内和类外均可以访问。

**private：** 只在类内和友元函数可以访问，类外不可访问。

**protect：**  类内和子类中可以访问，类外不可访问。

成员属性建议封装为私有，通过公有方法对外。


**继承方式：**


<div class="fig figcenter">
    <img src="https://img-blog.csdnimg.cn/a3d6685b18f248c0b67c30f39f410e69.png" width=500px>
    <div class="figcaption"> 继承方式</div>
</div>


## 构造函数和析构函数

**构造函数：** 用于成员属性的初始化，对象创建时系统自动调用。

**析构函数：** 对象销毁前系统执行一些清理工作。

- 构造函数和析构函数不返回任何值，也不返回void。
- 构造函数可以有参数，可以重载。
- 构造函数和类名相同。
- 析构函数不可以有参数，不可以重载。
- 析构函数和类名相同，前面加`~`

### 构造函数的种类：

- 根据参数划分：有参构造和无参构造

- 根据类型划分：普通构造和*拷贝构造*

> 注意: 不要用拷贝构造函数初始化匿名对象


### 构造函数的重载： 

构造函数可以各种重载，只需要注意别出现歧义即可，例如下面情况在初始化时就会出错，第二个有参构造函数给了默认值，因此不传任何参数也可以调用。这和无参构造函数发生了歧义：

```c
class Student
{
    public:
        string name;
        int age;

        Student();
        Student(string, int);
};

Student::Student()
{
    cout << "无参构造函数" << endl;
}

Student::Student(string name="xiaoming", int age=18)//这里全部加默认参数会和无参构造函数出现歧义
{
    cout << "有参构造函数" << endl;
    this->name = name;
    this->age = age;
}
```


### 对象的各种初始化方式
1. 括号法
	```c
	Student p1;
	Student p2("xiaoming", 18);
	Student p3(p2);
	```
	
2. 显示法
	```c
	Student p1;
	Student p2 = Student("xiaoming", 18);
	Student p3 = Student(p2);
	```

3. 隐式转换法（不够直观）

	```c
	Student p4 = {"xiaoming", 18};
	Student p5 = p4;
	```

完整示例：

```c
#include <iostream>
#include <string>

using namespace std;

class Student
{
    public:
        string name;
        int age;

        Student();
        Student(string, int);
        Student(const Student & p);
        ~Student();

        void set_age(int age);
        int get_age();
};

Student::Student()
{
    cout << "无参构造函数" << endl;
}

Student::Student(string name, int age)
{
    cout << "有参构造函数" << endl;
    this->name = name;
    this->age = age;
}

Student::Student(const Student & p)
{
    cout << "拷贝构造函数" << endl;    
}

Student::~Student()
{
    cout << "析构函数" << endl;
}

void Student::set_age(int age)
{
    this->age = age;
}

int Student::get_age()
{
    return this->age;
}

int main()
{
    //方法1
    Student stu1; //注意stu1不能加括号，否则编译器会将其看成函数声明
    //方法2
    Student stu2("xiaoming", 10);
    //方法3
    Student stu3 = Student();
    //方法4
    Student(); //匿名对象，当前行结束后，系统回收内存
    //方法5 new方法在堆内存创建对象
    Student * student = new Student("xiaoming", 20);
    delete student;
	//方法6
	Student stu6 = {"小明", 20};

    system("pause");
    return 0;
}
```

### 拷贝构造函数的调用时机：

- 使用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式给函数传递参数
- 以值的方式返回局部对象


### 构造函数的调用规则
- 默认情况下，创建一个类，c++编译器会给每个类添加至少三个函数
	- 默认构造函数（空实现）
	- 析构函数（空实现）
	- 拷贝构造函数（值拷贝）
- 如果自己写了有参构造，编译器不再提供默认构造，**但会提供拷贝构造函数**。
- 如果我们写了拷贝构造函数，则编译器不在提供任何构造函数了。


> **注意：在拷贝构造函数中，如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题。**


## 友元函数

关键字：`frind`

- 可以访问类中的私有成员

三种实现方式：

1. 全局函数做友元
2. 类做友元
3. 成员函数做友元



## This 指针

This指向被调用函数所属的对象。

- 解决名称冲突
- 返回对象本身用`*this`

## 静态成员

静态成员变量：

- 所有对象共享同一份数据
- 在编译阶段分配内存
- 类内声明，类外初始化

静态成员函数：

- 所有对象共享同一个函数
- 静态成员函数只能访问静态成员变量
- 静态成员函数没有this指针
- 在不创建对象时也可以使用





# 参考文献

- [C++ 教程](https://www.runoob.com/cplusplus/cpp-tutorial.html)
- [B站课程](https://www.bilibili.com/video/BV1et411b73Z?spm_id_from=333.337.search-card.all.click)
