# Python中的引用


<!--more-->

之前有一段时间被python的引用搞得头昏脑胀，学了之后现在再看已经记不太清了，于是特来记录一番.

## 一些基本概念

1. script  脚本是一个python文件，可以直接运行，但是一般不包含类呀class呀或者一些变量的定义，只是用来执行
2. module  模块是一个`变量，数组，函数和类的集合`.模块是一种以.py为后缀的文件，用于表示程序的一部分，模块的名称是该py文件的名称
3. package  包是指一个文件夹中包含很多module（几个模块的集合）。包体现了模块的机构化管理思想。老版本必须在文件夹下放一个__init__.py，不然不识别，现在可有可无，不过建议加上,该目录下的文件被视为一个单一的包。
```markdown
#目录结构
.
|-- creatures
|   |-- __init__.py
|   |-- ch.py
|   |-- moster.py
|-- magic
    |-- __init__.py
    |-- magic.py
```
4. library 一个库是几个包的集合。python中的库并没有具体的定义，着重强调其功能性。
5. framework 框架，框架与库类似，往往集成了多种库的功能。

## if __name__=='__main__':
假设在同一目录下有两个文件m1.py与m2.py
其中m1的内容如下：
```python
def f1():
  print("this is m1")
```
m2内容如下：
```python
from m1 import f1
f1()
```
执行m2.py，输出将为：
```shell
this is m1
this is m1
```
但是如果我们修改m1.py的内容为：
```python
def f1():
  print("this is m1")
if __name__=='__main__':
  f1()
```
那么此时再运行m2就会输出一个m1.

okay,现在来探究一下__name__.我们现在在m1.py的代码中加入一行代码：
```python
def f1():
  print("this is m1")
print(__name__)
if __name__=='__main__':
  f1()
```
此时再次执行m1.py，屏幕中将输出：
```shell
__main__
this is m1
```
再次执行m2.py,屏幕将输出：
```shell
m1
this is m1
```
也就是说在运行m1的时候`__name__`将等于`__main__`,而在运行m2时m1中的`__name__`变为了`m1`。

name这个变量编译器会自动定义，可以通过`print(dir())`显示。
修改m1的代码为：
```python
print(dir())
def f1():
  print("this is m1")
print(__name__)
a=34
print(dir())
if __name__=='__main__':
  f1()
```
执行m1，将输出：
```shell
['__annotations__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__']
['__annotations__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'a', 'f1']
this is m1
__main__
```
> 看到这里，我们要知道两个事情，第一个事情是我们在import一个模块时，会自动运行这个模块中的所有代码，这与引用的形式无关，如果你不想执行那么就使用__name__.第二点是不管import几次，同一个文件只会执行一次（解释器发现第一次已经加载了，第二次便不加载了）。

## sys
编写一段python代码如下：
```python
import sys
print(sys.path)
```
代码将输出
```shell
['/home/zzh/data/attack_based_steal/imp/package', '/opt/anaconda3/lib/python312.zip', '/opt/anaconda3/lib/python3.12', '/opt/anaconda3/lib/python3.12/lib-dynload', '/opt/anaconda3/lib/python3.12/site-packages']
```
这是一个列表，当你在运行一个代码时，python会在这些路径里查找你要引用的模块。
如果找不到要引用的那个包，那么有两种方法来解决。

现在的目录如下：
```shell
.
├── m1.py
├── m2.py
├── pkg
│   ├── __pycache__
│   │   └── run.cpython-312.pyc
│   └── run.py
└── __pycache__
    └── m1.cpython-312.pyc
```
如果想在m2.py中引用run.py，假设这样写：
```python
#m2.py
from m1 import f1
import m1
import run
import sys
print(sys.path)
f1()
```
代码将会报错，这是因为python在当前的path中找不到run，此时可以这样解决：
1. 使用绝对路径引用
```python
#m2.py
from m1 import f1
import m1
import pkg.run
import sys
print(sys.path)
f1()
```
这种方法可以成功是因为pkg位于根目录package下面，python可以找到pkg。

2. 可以手动将m2的路径添加到path中
```python
sys.path.append('/home/zzh/data/attack_based_steal/imp/package/pkg')
```

## 相对引用（容易犯错）
当前我们的文件目录如下：
```shell
.
└── package
    ├── m1.py
    ├── m2.py
    ├── pkg
    │   ├── __pycache__
    │   │   └── run.cpython-312.pyc
    │   ├── run2.py
    │   └── run.py
    └── __pycache__
        └── m1.cpython-312.pyc
```
此时四个代码文件依次如下：
```python
#m1.py
import sys
print(sys.path)
def f1():
  print("this is m1")
if __name__=='__main__':
  f1()

#m2.py
import sys
import m1
sys.path.append('/home/zzh/data/attack_based_steal/imp/package/pkg')
import pkg.run2


#run.py
def run1():
    print("this run1")

run1()

#run2.py
from  .run import run1

def run2():
    print("this run2")

run1()


```
此时运行m2.py，代码将输出：
```shell
['/home/zzh/data/attack_based_steal/imp/package', '/opt/anaconda3/lib/python312.zip', '/opt/anaconda3/lib/python3.12', '/opt/anaconda3/lib/python3.12/lib-dynload', '/opt/anaconda3/lib/python3.12/site-packages']
this run1
this run1
```
1. 第一个输出的列表是m2中引用m1时运行m1中的代码`print(sys.path)`输出的。
2. 第二个输出是run2引用run1时执行run1时得到的
3. 第三个输出时run2自己执行`run1()`得到的

:wink: 假设我们将m2的代码修改为：
```python
import sys
import m1
sys.path.append('/home/zzh/data/attack_based_steal/imp/package/pkg')
import run2
```
代码将直接报错：
```shell
['/home/zzh/data/attack_based_steal/imp/package', '/opt/anaconda3/lib/python312.zip', '/opt/anaconda3/lib/python3.12', '/opt/anaconda3/lib/python3.12/lib-dynload', '/opt/anaconda3/lib/python3.12/site-packages']
Traceback (most recent call last):
  File "/home/zzh/data/attack_based_steal/imp/package/m2.py", line 5, in <module>
    import run2
  File "/home/zzh/data/attack_based_steal/imp/package/pkg/run2.py", line 1, in <module>
    from  .run import run1
ImportError: attempted relative import with no known parent package
```
:wink: 注意这里报错并不是因为m2没有引用到run2。这里的报错与直接执行run2的报错相同。若直接执行run2，代码将报错：
```shell
Traceback (most recent call last):
  File "/home/zzh/data/attack_based_steal/imp/package/pkg/run2.py", line 1, in <module>
    from  .run import run1
ImportError: attempted relative import with no known parent package
```

> 这是因为如果使用绝对路径进行引用的时候python使用sys.path来查找模块，如果找不到就报错。但是相对引用是通过模块的__name__来查找的。

在run2中添加一行代码输出`__name__`:
```python
#run2.py
print(__name__)
from  .run import run1
def run2():
    print("this run2")
run1()
```
此时分别执行m2与run2，代码将分别输出：
```shell
pkg.run2

__main__
```
也就是说这两种方式下run2的`__name__`是不一致的。
那么好玩的就来了，如果我强行想避免这种报错怎么办呢？
修改run2.py的代码如下：
```python
print(__name__)
__name__ = 'pkg.abc'
import sys
sys.path.append('/home/zzh/data/attack_based_steal/imp/package')
from  .run import run1

def run2():
    print("this run2")
run1()

```
我们直接修改掉run2的`__name__`，同时将pkg所在目录添加到sys.path中，这样直接执行run2便不报错了。这是因为相对路径在查找时靠的时__name__,而此时的name告诉python上级目录是pkg，而pkg可以在path下找的，因此便不会再报错了。但是并不推荐这么写:wink:.


>实际上在真实的开发环境中一般也都是运行根目录下的主程序，而不会去直接运行包里面的文件。

再举一个例子，假如此时m2.py引用m1使用相对引用`from .m1 import *`那么也是会报错的，同理此时的m2的__name__是__main__,因此也找不到父目录。若要修改的话要将m2的__name__修改为'package.abc'然后再将package的上级目录添加到sys.path中。


## __init__.py

__init__.py 文件主要用来初始化，可以为空。
现有目录如下：
```shell
.
└── package
    ├── m1.py
    ├── m2.py
    ├── pkg
    │   ├── __init__.py
    │   ├── __pycache__
    │   │   ├── __init__.cpython-312.pyc
    │   │   ├── run2.cpython-312.pyc
    │   │   └── run.cpython-312.pyc
    │   ├── run2.py
    │   └── run.py
    └── __pycache__
        └── m1.cpython-312.pyc
```
其中`__init__.py`中的内容如下`from .run2 import run2`,那么此时我们就可以在m2.py中编写`from pkg import run2`。这样写的好处是如果pkg是你自己写的，那么你可以通过这个文件来告诉别人哪些东西可以用，而且也不需要使用者自行查找。

## subpackage
`.`表示当前目录那么`..`就表示上一级目录。

完结撒花:wink:

---

> Author: July  
> URL: http://localhost:1313/posts/2146d33/  

