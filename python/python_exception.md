---
layout: post 
---

# 简介
本文主要介绍python的异常处理机制，包括：
1. 如何使用异常处理
2. 异常的传递
3. 抛出异常

由于比较简单，因此这里介绍的不是很详细。

有一些代码来源于网络。

<!-- more -->
# 如何使用异常处理
所有的面向对象语言对异常的处理方式大同小异，在python中的处理异常的结构如下：

```python
try:
    # 可能出错的程序
except (Exception1, Exception2):     # 注意，python3中如果同时处理多个异常,需要将多个异常放在元组中。
    # 捕获到异常后对应的处理方法
except Exception as e:
    # 捕获到异常后对应的处理方法
    print(e)
    raise
else：
    # 如果没有捕获到异常对应处理方法
finally:
    # 触发异常或者不触发异常都需要处理的部分
```
接下来我们对上述结构逐一介绍。

## try ... except
`try`里面放可能出现问题的代码
`except`里面放捕获到错误之后的处理的方法

```python
try:
    print(num)
except NameError as e:
    print('捕获到的错误信息：',e)

# Output:
捕获到的错误信息： name 'num' is not defined
```

## except 捕获多个异常
直接在except将多个异常的名字放在元组里即可:

```python
try:
    open('hahaha')
except (NameError, IOError):
    print('catch exception')

# Output
catch exception
```

## 捕获所有异常
`Exception`类是所有异常类的父类，可以直接用`Exception`接收所有异常。
```python
try:
    print(num)
except Exception as e:
    print(e)

# Output:
 name 'num' is not defined
```

## else
当没有异常使，可以用`else`处理。
```python
try:
    num = 100
    print(num)
except Exception as e:
    print(e)
else:
    print('not catch IOError!')

# Output 
100
not catch IOError!
```

## try ... finally
无论检测到还是检测不到异常，有些代码都需要执行，此时放在`finally`里。
```python
import time
try:
    f = open('test.txt')
    try:
        while True:
            content = f.readline()
            if len(content) == 0:
                break
            time.sleep(2)
            print(content)
    except:
        #如果在读取文件的过程中，产生了异常，那么就会捕获到
        #比如 按下了 ctrl+c
        pass
    finally:
        f.close()
        print('关闭文件')
except:
    print("没有这个文件")
```

# 异常的传递
使用`try ... except`捕获异常还有一个好处，即异常可以跨越多层进行传递。如下面的例子：

```python
def test1():
    print("----test1-1----")
    print(num)
    print("----test1-2----")

def test3():
    try:
        print("----test3-1----")
        test1()
        print("----test3-2----")
    except Exception as result:
        print("捕获到了异常，信息是:%s"%result)

    print("----test3-3----")

test3()

#Output
----test3-1----
----test1-1----
捕获到了异常，信息是:global name 'num' is not defined
----test3-3----
```
这样我们就可以不必在每个可能出现错误的地方都写上`try`，而只需要在合适的层次进行处理即可。

# 抛出异常

## 抛出已有异常
可以通过`raise`来将捕获到的异常再抛出去。如下：
```python
class Test(object):
    def __init__(self, switch):
        self.switch = switch #开关
    def calc(self, a, b):
        try:
            return a/b
        except Exception as result:
            if self.switch:
                print("捕获开启，已经捕获到了异常，信息如下:")
                print(result)
            else:
                #重新抛出这个异常，此时就不会被这个异常处理给捕获到，从而触发默认的异常处理
                raise

a = Test(True)
a.calc(11,0)

print("----------------------华丽的分割线----------------")

a.switch = False
a.calc(11,0)

#Output 
捕获开启，已经捕获到了异常，信息如下:
integer division or modulo by zero
----------------------华丽的分割线----------------
Traceback (most recent call last):
  File "test.py", line 23, in <module>
    a.calc(11,0)
  File "test.py", line 7, in calc
    return a/b
ZeroDivisionError: integer division or modulo by zero
```
## 抛出自定义异常
自定义异常需要继承`Exception`类即可，如下：
```python
# -*- coding:utf8 -*- 
class NumError(Exception):
    def __init__(self, num, max_):
        self.num = num
        self.max_ = max_

try:
    num = 101
    if num > 100:
        raise NumError(num, 100)
except NumError as e:
    print('%d is too large, max is %d' %(e.num, e.max_))

# Output
101 is too large, max is 100
```
