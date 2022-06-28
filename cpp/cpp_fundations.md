---
layout: post
---

## 数组
一维数组求长度：

```c
double x[10];

sizeof(x) / sizeof(x[0]);
```

二维数组求行数：

```c
double x[5][5];
sizeof(x) / sizeof[x[0]];

sizeof(x) / sizeof(x[0][0]);
```

## 指针

在32位系统下，指针类型占4个字节空间大小，不管什么类型指针
在64位系统下，指针类型占8个字节空间大小，不管什么类型指针

### 空指针

**空指针** 指针变量指向内存中编号为0的空间，用于初始化指针。**空指针指向的内存是不能访问的**

**野指针** 指针变量指向非法的内存空间，尽量避免出现这种情况。

```c
//空指针定义
int * x = NULL;
```

### 指针运算

指针运算可以包括++, --, +, -，可以通过对指针递增进而访问数组元素，例如：

```c
int arr[5] = {1, 2, 3, 4, 5};
int *p = arr;

for(int i = 0; i < 5; i++)
{
    cout << *p << endl;
    p++;
}
```

指针的比较包括>, <, ==。

```c
int arr[5] = {1, 2, 3, 4, 5};

for(int *p = arr; p <= &arr[4]; p++)
{
    cout << *p << endl;
}
```

### 指针 vs 数组

数组的名称为一个指针常量，因此可以采用星号的形式对数组内数据进行修改，但不能修改数组的地址。如：

```c
int x[3] = {1, 2, 3};
*x = 0;
cout << *x << endl;
// x++; 报错
```

## 引用

引用必须初始化，并且一旦初始化之后就不能更改了。

和指针一样，引用在初始化时`&`符号位置较为灵活。如下三种方式等价：

```c
int x = 0;
int& y = x;
int & y = x;
int &y = x;
cout << y << endl;
```



引用常用的方式是作为函数的参数或返回值。


## 函数

函数调用的三种方式：传值调用，指针调用，引用调用。


传值调用时，形参不会改变实参的值。

指针调用时，形参会改变实参的值。因为将地址传给形参，相当于形参在直接操作实参。

引用调用时，形参同样会改变实参的值。


## 常量

### 定义常量

**使用 #define 预处理器。**

```c
#define LENGTH 10 
```

**使用const关键字**

```c
const int LENGTH 10;
```

> 注意：把常量定义为大写字母形式，是一个很好的编程实践。

### 指针常量和常量指针

指针常量：pointer to const
常量指针：const pointer

**const修饰谁主要看星号，const放在星号左边则修饰指向的对象，const在星号右边则修饰指针**

**对于名字的叫法，** 同样可以以星号为分界线，从后往前叫，必要时将星号翻译为`to`。比如`const int * p`从后往前命名为`pointer to const int`，意思是指向整形常量的指针，因此指针指向的值是不可以修改的；如果是`int * const p`则命名为`const point to int`，意思是指向整形的常量指针，指针的地址不可变，但指向的值可变。

根据上述规则，我们很容易分清语法的正误。如下：

例1：

```c
int a = 100;
int b = 200;
const int * p = &a; //指向整型常量的指针，因此指针指向的值不可以改
//int const * p = &a; 二者等价

p = &b; //正确
// *p = 20; 报错，
```

例2：

```c
int a = 100;
int b = 200;
int * const p = &a; //指向整型的指针常量，因此指针的指向不可以变。

//p = &b; //报错
*p = 20; //正确
```

最后，来个测试：

```c
1. const int p;	//整形常量p; const int
2. const int* p; //指向整形常量的指针p; pointer to const int
3. int const* p; //指向整型常量的指针p; pointer to const int
4. int * const p; //指向整形的常量指针p; const pointer to int
5. const int * const p; //指向整形常量的常量指针p; const pointer to const int
6. int const * const p; //指向整形常量的常量指针p; const pointer to const int
```

**总结一下可以发现**，二者的区别关键是以星号为分界线，看const修饰谁。


## 结构体

### 结构体定义

```c
struct Student
{
    string name;
    int age;
};

Student stu; //第一种初始化方式：C++
struct Student stu2; //第二种初始化方式：C或C++
```

在c++中，上述定义方式在初始化和函数传参是采用第一种方式即可；但是在c语言中，必须使用第二种方式。因此，为了兼容c语言，使结构体在初始化时省略`struct`关键字，可以采用如下定义方式：

```c
typedef struct Student
{
    string name;
    int age;
} Student;
```

这样，后面的Student相当于struct Student的别名，初始化时直接采用Student即可。

定义完结构体之后，后面的使用方法就和其他变量类型基本相同了。

### 指向结构体的指针

指向结构体的指针通过`->`访问结构体的内部成员。例：

```c
Student stu;
stu.name = "test";
stu.age = 17;

Student *p = &stu;

cout << p->name << endl << p->age << endl;
```

## 动态内存

栈：在函数内部声明的所有变量都将占用栈内存。
堆：程序中未使用的内存，在程序运行时可用于动态分配内存。采用new运算符动态分配堆内存，采用delete运算符释放堆内存。如：

```c
   //整型数据的动态内存分配
    int * p = NULL;
    p = new int;
    *p = 100;
    cout << *p << endl;
    
    delete p;

    //数组的动态内存分配
    int * p1 = NULL;
    p1 = new int[3];
    for(int i = 0; i < 3; i++)
    {
        p1[i] = i * i;
    }
    for(int i = 0; i < 3; i++)
    {
        cout << p[i] << endl;
    }

    delete[] p1;
```

## 参考文献

- [指针常量和常量指针的区别](https://blog.csdn.net/weixin_39992803/article/details/111571719)

- [C++ 教程](https://www.runoob.com/cplusplus/cpp-tutorial.html)

- [B站课程](https://www.bilibili.com/video/BV1et411b73Z?spm_id_from=333.337.search-card.all.click)
