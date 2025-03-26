---
title: "Python对象"
subtitle: ""
date: 2025-03-19T13:19:16+08:00
description: ""
keywords: ""
comment: false
---

本文主要介绍python中的类，当然本目录主要是讲述python开发，在这里记录python中的类主要是由于作者本人未对python中的类掌握的足够好。

## 类
封装，继承与多态是面向对象编程最核心的概念。

### 类的定义

在python中使用`class`关键字定义一个类，类中可以包含属性与行为，一个简单的示例如下：

```python
class Test():
    def __init__(self):
        self.name = "仅仅是一个测试"

    def out_name(self):
        print(self.name)


if __name__ == "__main__":
    t = Test()
    print(t.name)
    t.out_name()

```

这段代码将有如下输出：
```python
仅仅是一个测试
仅仅是一个测试
```

`self`是python中的关键字，这个关键字在写方法时必须存在。

### 构造方法
`__init__()`方法是构造方法，在创建类对象时会自动执行。

### 魔术方法
`__init__()`是python类内置的方法之一，这些内置的类方法各自有各自特殊的功能，这些内置方法就是“魔术方法”。

### 封装

#### 私有成员变量与私有成员方法
定义私有成员的方法非常简单，只需要在（无论变量还是方法）前面加两个下划线即可。
编写如下代码：

```python
class Test():
    father = "z"
    __mather = "p"

    def __init__(self):
        self.name = "仅仅是一个测试"

    def out_name(self):
        print(self.name)


if __name__ == "__main__":
    t = Test()
    print(t.name)
    t.out_name()
    print(t.father)
    print(t.mather)
```
运行之后将会报错：
```python
Traceback (most recent call last):
  File "C:\Users\76290\PycharmProjects\pythonProject1\test.py", line 17, in <module>
    print(t.mather)
          ^^^^^^^^
AttributeError: 'Test' object has no attribute 'mather'. Did you mean: 'father'?
仅仅是一个测试
仅仅是一个测试
z

```

不过仍然可以通过别的方式访问：
```python
class Test():
    father = "z"
    __mather = "p"

    def __init__(self):
        self.name = "仅仅是一个测试"

    def out_name(self):
        print(self.name)

    def out_mather(self):
        print(self.__mather)


if __name__ == "__main__":
    t = Test()
    print(t.name)
    t.out_name()
    print(t.father)
    t.out_mather()
```
这段代码将输出：
```python
仅仅是一个测试
仅仅是一个测试
z
p
```
也就是说类里面的其他成员是可以使用这些私有变量和方法的。

### 继承
继承的写法如下：
```python
class Test():
    father = "z"
    __mather = "p"

    def __init__(self):
        self.name = "仅仅是一个测试"

    def out_name(self):
        print(self.name)

    def out_mather(self):
        print(self.__mather)


class Test2(Test):
    grandfather = "zz"

```
`class 类名(父类名)`这称为单继承。

python同样支持多继承，即一个类继承多个父类。

`class 类名(父类1，父类2，父类3)`.如果父类有重名变量的话，那么左边的优先，即谁先来的谁优先级高。

继承也支持复写，可以直接将父类的属性覆盖掉。

#### 调用父类同名成员
一旦复写父类成员，那么类对象调用成员的时候，就会调用复写后的新成员。如果需要使用被复写的父类的成员，需要特殊的调用方式：
1. 父类名.成员变量   父类名.成员方法(self)
2. 使用super()调用父类成员   super().成员变量  super.成员方法()
注意方法一需要传入self，方法二不需要传入self。

#### 类型注解
类型注解可以帮助IDE对代码进行类型推断，协助做代码提示。

类型注解分为两种：
1. 变量的类型注解
2. 函数（方法）形参列表和返回值的类型注解

基础语法就是 `变量：类型`
比如：
```python
var1: int = 10
```

### 多态
多态指的是完成某个行为时使用不同的对象会得到不同的状态。
举一个例子：
```python
class Animal:
    def speak(self):
        pass


class Dog(Animal):
    def speak(self):
        print("www")


class Cat(Animal):
    def speak(self):
        print("mmm")


if __name__ == "__main__":
    def make_noise(animal: Animal):
        animal.speak()


    dog = Dog()
    cat = Cat()

    make_noise(dog)
    make_noise(cat)

```
运行这段代码将输出：
```shell
www
mmm
```
根据传入的对象不同得到的输出也不同。

多态的另一个用处是`抽象类(接口)`，比如上一段代码中的Animal类的speak方法是一个`pass`，这样设计的含义是父类用来确定有哪些方法，具体如何实现由子类自行决定。这种写法，就叫做抽象类（也可以称之为接口），此外，含有抽象方法的类称为抽象类，方法体是空实现（pass）的称之为抽象方法。





### 一些小知识点
- Q1 class Test()与class Test有什么区别？
在python3中并无区别，在python3中推荐省略括号，仅在需要继承其他类时使用括号。