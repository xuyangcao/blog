---
layout: post 
---

<!-- vim-markdown-toc Marked -->

* [前言](#前言)
* [序列式容器](#序列式容器)
  * [vector](#vector)
  * [deque](#deque)
  * [list](#list)
  * [array](#array)
  * [forward_list](#forward_list)
* [关联式容器](#关联式容器)
  * [set](#set)
  * [multiset](#multiset)
  * [map](#map)
  * [multimap](#multimap)
* [无序关联式容器](#无序关联式容器)
  * [unordered_set](#unordered_set)
  * [unordered_multiset](#unordered_multiset)
  * [unordered_map](#unordered_map)
  * [unordered_multimap](#unordered_multimap)
* [容器适配器](#容器适配器)
  * [stack](#stack)
  * [quque](#quque)
  * [priority_queue](#priority_queue)
* [参考资料](#参考资料)

<!-- vim-markdown-toc -->

## 前言

- [STL容器官方文档](http://cplusplus.com/reference/stl/)

- 容器是C++ Standard Template Library(STL)内的一部分。

- 容器可以划分为

    - 序列式容器: 强调值的排序，每个元素均有固定位置。底层是线性序列的数据结构，里面存储的是元素本身。包括:
        - array
        - vector
        - deque
        - forward_list
        - list
    - 关联式容器：底层为树形结构，存储<key, value>结构的键值对,包括:
        - set
        - multiset 
        - map 
        - multimap
    - 无序关联式容器：底层为hash结构，包括:
        - unordered_set 
        - unordered_multiset 
        - unordered_map 
        - unordered_multimap
    - 容器适配器: 容器适配器不是完整的容器类，而是提供特定接口的类，且这些结构依赖于上述容器类之一的对象来处理元素。包括:
        - stack
        - queue 
        - priority_queue

--- 

>    - STL的产生：为提高代码复用性，提供标准的数据结构和算法
>
>    - **广义上，STL分为**
>
>        - 容器(container)
>        - 算法(algorithm)
>        - 迭代器(iterator)
>
>    - 容器和算法之间通过迭代器进行无缝连接
>
>    - STL几乎所有代码都采用了模板类或模板函数
>
>    - STL六大组件：
>
>        - 容器: 各种数据结构， vector, list, map, set
>        - 算法: 各种常用算法，如sort， find
>        - 迭代器: 容器和算法之间的胶合剂, 算法通过迭代器访问容器中的数据
>        - 仿函数: 行为类似函数，可作为算法的某种策略
>        - 适配器: 修饰容器接口
>        - 空间配置器：空间的配置和管理

---


## 序列式容器

### vector

- [vector官方文档](http://cplusplus.com/reference/vector/vector/vector/)

- 基本用法(定义，赋值，遍历)

    ```cpp
    // 构造
    vector<int> first;                                // empty vector of ints
    vector<int> second (4,100);                       // four ints with value 100
    vector<int> third (second.begin(),second.end());  // iterating through second
    vector<int> fourth (third);

    //赋值
    vector<int> vec;
    vec.push_back(10);
    vec.push_back(20);
    vec.push_back(30);
    vec.push_back(40);

    // 通过迭代器访问容器中的数据
    vector<int>::iterator iter_begin = vec.begin(); //起始迭代器，指向容器中第一个元素
    vector<int>::iterator iter_end = vec.end(); //结束迭代器，指向容器中最后一个元素的下一个位置

    // 第一遍历方式
    while (iter_begin != iter_end) {
        cout << *iter_begin << endl;
        iter_begin++;
    }

    //第二种遍历方式,较常用
    for (vector<int>::iterator it=vec.begin(); it != vec.end(); it++) {
        cout << *it << endl;
    }

    //第三种方式，利用STL中的遍历算法
    for_each(vec.begin(), vec.end(), m_print);

    //第四种方式， c++ 11以后，可以采用如下便捷方式调用
    for (int num : vec) {
        cout << num << endl;
    }

    //第五种方式
    for (int i = 0; i < vec.size(); i++) {
        cout << vec[i] << endl;
    }
    ```

- 和数组非常相似，但空间和动态扩展。
- **动态扩展**不是在原有空间扩展，而是找更大的内存空间，将原数据拷贝到新空间中，再释放原空间。
- 迭代器支持随机访问的迭代器

- 巧妙利用`swap`函数可以缩减内存空间,如下：

    ```c
    vector<int> vec(100000, 0);
    cout << vec.capacity() << " " << vec.size() << endl;

    vec.resize(3); //resize之后空间被浪费了
    cout << vec.capacity() << " " << vec.size() << endl;

    vector<int>(3).swap(vec);
    cout << vec.capacity() << " " << vec.size() << endl;
    ```

- `reserve`函数可以减少动态开辟内存的次数

    ```c
    vector<int> vec;
    vec.reserve(100000);

    int* p = NULL;
    int num = 0;
    for (int i = 0; i < 100000; i++) {
        vec.push_back(10);
        if (p != &vec[0]) {
            num += 1;
            p = &vec[0];
        }
    }
    cout << num << endl;
    ```

### deque

### list

### array

### forward_list


## 关联式容器

### set

- [set官方文档](http://m.cplusplus.com/reference/set/set/)

- set 插入元素时，只需插入value即可，但其底层存储的是键值对。

- set 底层采用红黑树实现

- set 中的元素不可重复

- 基本用法: 

    ```cpp
    #include <iostream>
    #include <vector>
    #include <set>

    using namespace std;

    void show(set<int> s) {
        for (int num : s) {
            cout << num << " ";
        }
        cout << endl;
    }

    int main() {
        set<int> set1;
        // 所有元素插入时会自动排序，不允许插入相同的值
        set1.insert(10);
        set1.insert(30);
        set1.insert(40);
        set1.insert(40);
        set1.insert(20);
        show(set1); // output: 10 20 30 40
        
        // 拷贝构造
        set<int> set2(set1);

        //接口测试
        cout << set2.size() << endl;
        cout << set2.empty() << endl;
        cout << *(set2.find(10)) << endl;
        cout << (set2.find(10) == set2.end()) << endl;
        cout << (set2.find(50) == set2.end()) << endl;

        set2.erase(set2.begin());
        set2.erase(10);
        set2.erase(20);
        show(set2);

        set2.clear();
        show(set2);

        return 0;
    }
    ```
- set内的数据默认从小到大排序
- set内存储自定义对象时，需要指定排序规则 

    ```c
    #include <iostream>
    #include <set>
    #include <string>

    using namespace std;

    class Student {
        public:
            string name;
            int age;

            Student(string name_, int age_) : name(name_), age(age_) {  };
    };

    class StudentCompare {
        public:
            bool operator()(const Student &stu1, const Student &stu2) {
                return (stu1.age < stu2.age); // 按年龄从小到大排序
            }
    };

    int main () {
        set<Student, StudentCompare> set1;
        set1.insert(Student("xiaoming", 18));
        set1.insert(Student("haha", 19));
        set1.insert(Student("hehe", 12));
        set1.insert(Student("heihei", 14));

        for (auto stu : set1) {
            cout << "name: " << stu.name << "\t" << "age: " << stu.age << endl;
        }

        return 0;
    }
    ```

### multiset

- [multiset官方文档](cplusplus.com/reference/set/multiset/)

- multiset 可以插入重复的数据

- 默认元素从小达到排列

- 基本使用方法和set相同:

    ```cpp
    #include <iostream>
    #include <vector>
    #include <set>

    using namespace std;

    void show(multiset<int> s) {
        for (int num : s) {
            cout << num << " ";
        }
        cout << endl;
    }

    int main() {
        multiset<int> set1;
        // 所有元素插入时会自动排序，不允许插入相同的值
        set1.insert(10);
        set1.insert(30);
        set1.insert(40);
        set1.insert(40);
        set1.insert(20);
        show(set1); // output: 10 20 30 40 40
        
        // 拷贝构造
        multiset<int> set2(set1);

        //接口测试
        cout << set2.size() << endl; //5
        cout << set2.empty() << endl; // 0
        cout << *(set2.find(10)) << endl; // 10
        cout << (set2.find(10) == set2.end()) << endl; // 0
        cout << (set2.find(50) == set2.end()) << endl; // 1 
        cout << set2.count(40) << endl; // 2

        set2.erase(set2.begin());
        set2.erase(10);
        set2.erase(20);
        show(set2); // 30 40 40

        set2.clear();
        show(set2); //空

        return 0;
    }
    ```

### map

- map 中所有元素都是pair, 其中第一个元素为key, 第二个元素为value。

- 元素按照key值自动排序

- 底层实现为二叉树

- 优点: 根据key值快速找到value值

- 不允许容器中有重复的key值

> 对组: pair<type, type> p(value1, value2);
> 功能: 表示成对出现的数据
> 定义中pair为结构体
> 基本使用方法：
> ```c
> pair<string, int> p("xiaoming", 2);
> cout << p.first << " " << p.second << endl;
> 
> pair<string, int> p2 = make_pair("xiaoli", 18);
> cout << p2.first << " " << p2.second << endl;
> ```

- map 的基本使用方式:

    ```cpp
    #include <iostream>
    #include <vector>
    #include <set>
    #include <map>

    using namespace std;

    void show(map<string, int> s) {
        for (auto num : s) {
            cout << num.first << ": " << num.second << endl;;
        }
        cout << endl;
    }

    int main() {
        map<string, int> map1;
        map1.insert(pair<string, int>("xiaoming", 18)); //插入一个匿名pair
        map1.insert(pair<string, int>("xiaoli", 10));
        map1.insert(pair<string, int>("xiaohua", 100));
        map1.insert(pair<string, int>("zhaoyun", 19));
        map1.insert(pair<string, int>("haha", 19));
        show(map1); //可以发现按照key从小到大排列

        //拷贝构造
        map<string, int> map2(map1);
        show(map2);

        //赋值
        map<string, int> map3;
        map3 = map2;
        show(map3);

        //接口测试
        cout << map3.size() << endl; // 5
        cout << (map3.find("xiaoming"))->second << endl;  //18
        cout << (map3.find("xiaoming") != map3.end()) << endl; // 1
        cout << (map3.count("xiaoming")) << endl; // 1

        map3.erase(map3.begin());
        map3.erase("xiaoming");
        show(map3);

        map3.clear();
        show(map3);

        return 0;
    }
    ```

- 利用仿函数改变排序规则

    ```cpp
    #include <iostream>
    #include <vector>
    #include <set>
    #include <map>

    using namespace std;

    class MapCompare {
        public:
            bool operator()(string s1, string s2) {
                return s1 > s2;
            }
    };

    void show(map<string, int, MapCompare> s) {
        for (auto num : s) {
            cout << num.first << ": " << num.second << endl;;
        }
        cout << endl;
    }

    int main() {
        map<string, int, MapCompare> map1;
        map1.insert(pair<string, int>("xiaoming", 18));
        map1.insert(pair<string, int>("xiaoli", 10));
        map1.insert(pair<string, int>("xiaohua", 100));
        map1.insert(pair<string, int>("zhaoyun", 19));
        map1.insert(pair<string, int>("haha", 19));
        show(map1);

        return 0;
    }
    ```

### multimap

- multimap可以存储相同key值的元素
- 其余基本和map相同

    ```cpp
    #include <iostream>
    #include <vector>
    #include <set>
    #include <map>

    using namespace std;

    void show(multimap<string, int> s) {
        for (auto num : s) {
            cout << num.first << ": " << num.second << endl;;
        }
        cout << endl;
    }

    int main() {
        multimap<string, int> map1;
        map1.insert(pair<string, int>("xiaoming", 18));
        map1.insert(pair<string, int>("xiaoming", 18));
        map1.insert(pair<string, int>("xiaoming", 19));
        map1.insert(pair<string, int>("xiaoming", 20));
        map1.insert(pair<string, int>("xiaoming", 21));
        map1.insert(pair<string, int>("xiaoli", 10));

        show(map1);
        cout << (map1.count("xiaoming")) << endl; // 5

        return 0;
    }
    ```

## 无序关联式容器

### unordered_set
### unordered_multiset
### unordered_map
### unordered_multimap

## 容器适配器

### stack 
### quque
### priority_queue


## 参考资料

- [cplusplus.com](http://cplusplus.com/reference/stl/)
- [B站课程](https://www.bilibili.com/video/BV1et411b73Z?spm_id_from=333.337.search-card.all.click) 


