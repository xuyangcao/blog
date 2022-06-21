---
layout: post
---

# 引言

简单的说，一个python文件就是一个模块，本文主要介绍以下3点：

- 模块的建立及导入

- 包的建立及导入

- 发布和安装自定义模块


# 模块的建立及导入  

我们在写c，或者c++时候，为了复用代码，总是将一系列相关的函数写在一个`.c`文件中，或者封装一个类写在一个`.cpp`文件中，方便其他程序调用。

python这里的模块起到同样的作用，我们可以把实现相同任务的一些类和函数写在一个`.py`文件中，称之为一个模块，方便其它程序调用。

## 新建一个模块

新建一个模块，也就是新建一个`.py`文件。

我们新建一个模块`utils.py`如下:

```python 
# utils.py test1和test2是我希望被别人重复利用的代码
def test1():
    print('-----test1-----')

def test2():
    print('-----test2-----')

```

## 导入一个模块
导入一个模块常用的几种方式如下：

```python
# 第一种
import utils 
utils.test1()


# 第二种
from utils import test1
test1()

# 第三种
from utils import test1, test2
test1()
test2()

# 第四种
import utils as u
u.test1()

# 第五种,这种方式建议少用
from utils import *
test1()
test2()
```

上述方式都可以完成模块的导入，但是最后一种方式不建议使用。`因为`如果当两个模块中有同名的函数时，后面导入的文件会覆盖掉前面的文件，这是就很难分清到底使用的是哪个模块中的方法了。

举个实际中的例子：

```python
import os 
import sys 
import numpy as np 
from skimage.io import imread, imsave
```

无论是新建一个模块和导入一个模块看起来都不难，接下来介绍一些需要注意的事情。

##  模块中的\_\_name\_\_ 

**当一个程序导入另一个模块时，实际上会把这个模块从头到尾执行一遍。** 

这样就带来个一个问题：一般情况下，我在模块中为了测试每个函数的功能是否正常，需要添加一些测试的程序，如果不把这些程序去除，在被其它程序导入时就会执行了这些程序，显然这是我们不希望看到的。

`__name__`的使用在不删除模块内测试代码的前提下解决了上述问题。我们先来做个测试，在`utils.py`模块中打印一个变量`__name__`如下：

```python
# utils.py
def test1():
    print('-----test1-----')

def test2():
    print('-----test2-----')

print(__name__)
```

```python
# main.py
import utils # 注意，这里我除了导入模块之外其他什么也没做
             # 由于导入模块时会把整个模块文件执行一遍，因此会直接调用print(__name__)这句话。

```

当我执行如下两条命令时，请注意`__name__`的值是如何变化的：

```bash
>>> python utils.py
>>> __main__
>>>
>>> python main.py
>>> utils
```

通过上述执行过程我们可以看出，当执行`utils.py`文件时，`__name__`的值为`'_main__'`,当执行`main.py`时，`utils.py`里面`__name__`的值为`'utils'`。因此我们只需要在`utils.py`的测试代码中加上如下代码即可：

```python
# utils.py
def test1():
    print('-----test1-----')

def test2():
    print('-----test2-----')

if __name__ == '__main__':
    test1()
    print(__name__)
```

上述判断语句添加完之后，当再有其他程序导入`utils`模块时，就不会执行`utils`里面测试代码的部分了。当直接执行`utils.py`文件时，才会执行测试代码的部分。

我们总结一下，一般来说，一个标准一点的`python`程序具有如下的格式：

```python
import  xxxx

class ClassName(object):
    def __init__():
        pass 

    def func1():
        pass 
    def 

def func():
    pass 

def func2():
    pass 

def main():
    pass 

if __name__ == '__main__':
    main() #注意这里的main()和c语言中的不一样，只是借用了C语言中的语境，表达一个程序的开始而已。
```

## 模块中的\_\_all\_\_
再回顾一下我们的`utils.py`模块

```python
# utils.py test1和test2是我希望被别人重复利用的代码
def test1():
    print('-----test1-----')

def test2():
    print('-----test2-----')
```

上述模块在使用`from utils import *`的方式被导入时候，所有的函数都会被导入，**如果我不希望部分函数被外部使用，而只是公开我想要给大家使用的代码，可以使用`__all__`变量来完成**，其使用方式如下：

```python
# utils.py
__all__ = ['TestClass', 'test1'] # 这里把函数，类的名字当成一个元素放在列表里。
                                 # 当其他函数使用from utils import *时，只能访问到`__all__`变量里面的内容
                                 # 这里test2函数就不能被其他模块使用了
class TestClass(object):
    pass 

def test1():
    print('-----test1-----')

def test2():
    print('-----test2-----')

if __name__ == '__main__':
    pass 
```

此时当`main.py`再次调用`utils`模块时候，就不能访问`test2`函数了。测试如下：

```python
In [1]: from utils import *

In [2]: test1()
-----test1--------

In [3]: test2()
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-3-35ebc1c532fc> in <module>()
----> 1 test2()

NameError: name 'test2' is not defined

In [4]: TestClass()
Out[4]: <utils.TestClass at 0x7fcf5055ae90>
```


>> 一个需要注意的地方是：`__all__`变量只用来约束`from xxx import *`的形式,而其他形式还是可以正常调用的。

如：

```python
In [10]: from utils import test2

In [11]: test2()
------test2--------
```

# 包的建立及导入

如果想快速了解使用方法，请直接看[总结部分](##2.4)。

## 包的建立

为了将具有相关联功能的模块放在一起管理，将这些模块放在同一个文件夹下。
比如有`sengMsg.py`和`revMsg.py`两个模块，由于他们都是和消息相关的模块，因此我用`msg`文件夹来把它们放在一起。如下：

```bash
msg/
├── revMsg.py
└── sendMsg.py
```

此时我们还不能直接通过`import`的方式直接使用msg这两个模块。比如`import msg`是无法使用里面的模块的。
>> 注意：`在python3`中，直接调用`import msg`不会出错，但是却无法导入模块并使用模块里面的功能。

```python
# python3
In [1]: import msg

In [2]: msg.revMsg.test1()
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-2-4c1d6211c490> in <module>()
----> 1 msg.revMsg.test1()

AttributeError: module 'msg' has no attribute 'revMsg'
```

>> 在`python2`中，即使是调用`import msg`也会直接报错，因为它们都将`msg`当做一个普通的文件夹处理

```python
# python2
[1]: import msg
---------------------------------------------------------------------------
ImportError                               Traceback (most recent call last)
<ipython-input-12-40dac918f9d8> in <module>()
----> 1 import msg

ImportError: No module named msg
```

## \_\_init\_\_.py文件
为了解决上述问题,需要在`msg`文件夹下新建一个`__init__.py`,告诉python解释器不要把`msg`当成一个普通的文件夹处理，而当做一个**包**对待。
>> 注意：`python3`在没有`__init__.py`文件时已经默认`msg`是一个包，因此上面一段话仅对`python2`来说有用。

此时我们可以认为**含有一个`__init__.py`文件，并且具有很多模块的一个文件夹，这个整体叫做包**。

```bash
msg/
├── __init__.py
├── revMsg.py
└── sendMsg.py
```

此时，我们就可以直接导入里面的模块了。

```python
In [1]: from msg import revMsg

In [2]: revMsg.test1()
----test1 revMsg----
```

但是如果通过如下方式导入时即

```python
In [1]: import msg

In [2]: msg.sengMsg.test1()
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-2-be5fc6eec2f7> in <module>()
----> 1 msg.sengMsg.test1()

AttributeError: 'module' object has no attribute 'sengMsg'
```

还是无法导入。和模块一样，当一个包被导入的时候，里面的`__init__.py`文件会被执行一遍，因此需要在里面添加如下内容，从而使得我们能够去除掉述导包时出现的错误：

```python
# __init__.py
from . import (sendMsg, revMsg) # 这种方式是python2和python3兼容的，因此建议使用这种方式。
```

此时就可以了：

```python
In [2]: import msg

In [3]: msg.revMsg.test1()
----test1 revMsg----
```

## \_\_all\_\_变量

和模块中的`__all__`变量一样，我们可以在`__init__.py`中写入`__all__ = ['xxx', 'xxx']`的形式定义`import *`时候可以使用哪些模块。

```python
# __init__.py
__all__ = ['revMsg']
```

此时通过`from msg import *`方式导入模块只能是`revMsg`模块。

>> 注意：`__all__`变量同样不会影响`from msg.sendMsg import test1`等类似方式的调用。只会影响到`import *`方式的调用。


>> **我们总结一个`__init__.py`文件下最好有两个内容：**
>> 1. `__all__`变量指定`from xxx import *`时候可以使用哪些模块。
>> 2. `from .xx import (xxx, xxxx, xxxx)`的内容，指定其他时候可以使用哪些模块。


# 总结

关于模块和包的创建以及导入我们总结如下：

1. 一个`xx.py`文件被称为一个模块。

2. 将实现相关联功能的模块放在一起，再添加一个`__init__.py`文件，放在一个文件夹中，称之为一个包。

3. `__all__`变量在模块中可以定义导入该模块时使用`from xx import *`时导入模块内的哪些内容。

4. `__all__`变量在`__init__.py`文件中可以定义使用`from xx import *`时导入哪些模块。

5. 当导入模块时，模块的对应文件会被从头到尾执行一遍，因此需要使用`__name__`变量的性质屏蔽掉测试代码。

6. 当导入包时，里面的`__init__.py`文件会从头到尾被执行一遍，因此可以在这里加上导包的命令使得当一个程序导入该包时可以导入包里面的模块。

7. 当然`__init__.py`也可以是一个空文件，只不过此时不能通过只导一个包的形式(`import xxx`)来调用包里面的模块(`xxx.xxx_model`).


最后我们在来看一个`skimage`中的其中一个包是怎么写的：

```python
# xxx/site-packages/skimage/segmentation/__init__.py
from .random_walker_segmentation import random_walker #表示从当前文件夹下的random_walker_segmentation模块中导入random_walker函数
from .active_contour_model import active_contour
from ._felzenszwalb import felzenszwalb
from .slic_superpixels import slic
from ._quickshift import quickshift
from .boundaries import find_boundaries, mark_boundaries
from ._clear_border import clear_border
from ._join import join_segmentations, relabel_from_one, relabel_sequential
from ..morphology import watershed # 表示从上一层路径下的morphology包中导入watershed模块

# 当使用from segmentation import *时，下面这些函数可以被使用
__all__ = ['random_walker'， 
           'active_contour',
           'felzenszwalb',
           'slic',
           'quickshift',
           'find_boundaries',
           'mark_boundaries',
           'clear_border',
           'join_segmentations',
           'relabel_from_one',
           'relabel_sequential',
           'watershed']
```


# 发布和安装自定义模块
这里只介绍一种通过`setup.py`文件制作包的方式，不能通过`pip`或`conda`命令安装，但是基本上对于目前的应用来说已经够用了。因此其他方式不再介绍。

## 制作
1. 首先确保包的目录结构如下：
   
    ```bash
    .
    ├── msg
    │   ├── __init__.py
    │   ├── revMsg.py
    │   └── sendMsg.py
    └── setup.py
    ```

2. 然后编辑`setup.py`文件如下：
   
    ```python
    from distutils.core import setup

    setup(name = 'xuyangcao', 
            version = '1.0', 
            description = 'xuyangcao\'s model',
            author = 'xuyangcao',
            py_modules = ['msg.sendMsg', 'msg.revMsg'])
    ```

3. build
    执行`setup.py`构建模块，命令如下：

    ```bash
    python setup.py build
    ```

    执行后其目录结构变化如下：


    ```bash
    .
    ├── build
    │   └── lib
    │       └── msg
    │           ├── __init__.py
    │           ├── revMsg.py
    │           └── sendMsg.py
    ├── msg
    │   ├── __init__.py
    │   ├── revMsg.py
    │   └── sendMsg.py
    └── setup.py
    ```

    发现build 文件夹下的东西就是原始文件夹里面内容的一个拷贝。

4. sdist
    `build`结束后，再执行如下命令：

    ```bash
    python setup.py sdist
    ```

    运行完之后目录结构如下：


    ```bash
        .
    ├── build
    │   └── lib
    │       └── msg
    │           ├── __init__.py
    │           ├── revMsg.py
    │           └── sendMsg.py
    ├── dist
    │   └── xuyangcao-1.0.tar.gz
    ├── MANIFEST
    ├── msg
    │   ├── __init__.py
    │   ├── revMsg.py
    │   └── sendMsg.py
    └── setup.py
    ```

    我们需要的东西就在`./dist`下面的压缩包里。

## 发布与安装

**发布**: 

将`./dist/xuyangcao-1.0.tar.gz`解压，发布到需要的地方即可。

**安装**

这个应该是比较熟悉的了，我们安装其他第三方模块时肯定也用过这种方式。

解压缩压缩包里的文件，其目录结构如下：

```bash
.
├── build
│   └── lib
│       └── msg
│           ├── __init__.py
│           ├── revMsg.py
│           └── sendMsg.py
├── msg
│   ├── __init__.py
│   ├── revMsg.py
│   └── sendMsg.py
├── PKG-INFO
└── setup.py
```

接下来就可以安装了，直接输入我们熟悉的命令：

```bash
python setup.py install
```

大功告成！

## 测试一下吧

```python
In [2]: import msg

In [3]: msg.revMsg.test1()
----test1 revMsg----
```

成功！
