# Python迭代器



# Python 迭代器详解

## 目录
1. [基本概念](#基本概念)
2. [创建迭代器](#创建迭代器)
3. [迭代器特性](#迭代器特性)
4. [常见迭代器类型](#常见迭代器类型)
5. [迭代器协议](#迭代器协议)
6. [注意事项](#注意事项)
7. [实际应用](#实际应用)
8. [最佳实践](#最佳实践)

---

## 基本概念

### 迭代器 vs 可迭代对象
| 特征                | 可迭代对象 (Iterable)          | 迭代器 (Iterator)          |
|---------------------|-------------------------------|---------------------------|
| 定义                | 可返回迭代器的对象             | 实现`__iter__`和`__next__`的对象 |
| 内存消耗            | 通常存储完整数据              | 按需生成数据，节省内存      |
| 状态保持            | 无状态                        | 保持遍历状态              |
| 示例                | list, tuple, dict, str        | generator, file object    |

### 迭代器核心特征
- **一次性消费**：遍历后无法重置
- **惰性求值**：按需生成元素
- **内存高效**：适合处理大数据集

---

## 创建迭代器

### 1. 内置函数转换

```python
numbers = [1, 2, 3]

my_iter = iter(numbers) # 创建迭代器
```

## 迭代器只能遍历一次

我们编写如下代码：
```python
a = [1,2,3]
b = iter(a)

for i in b:
    print(i)

print("*****")

for i in b:
    print(i)

```

运行该代码该代码将输出：
```shell
1
2
3
*****
```
你会发现在第二次输出时并没有任何输出。


## 常见迭代器类型

| 类型                | 描述                          | 注意事项                  |
|---------------------|-------------------------------|-------------------------|
| 文件对象            | `open()`返回的对象            | 读取后需重新打开         |
| 生成器              | 使用`yield`的函数             | 自动实现迭代器协议       |
| map/filter          | 函数式编程操作结果            | 结果只能遍历一次         |
| zip/enumerate       | 组合迭代工具                  | 按最短输入截断          |
| csv.reader          | CSV文件读取器                 | 需转换为列表持久化数据   |

---

## 迭代器协议

### 必须实现的方法
1. `__iter__()`: 返回迭代器自身
2. `__next__()`: 返回下一个元素，耗尽时抛出`StopIteration`


比如一个自己实现的迭代器如下：
```python
class CountDown:

    def __init__(self, start):

        self.current = start


    def __iter__(self):

        return self


    def __next__(self):

        if self.current <= 0:

            raise StopIteration

        num = self.current

        self.current -= 1

        return num

for n in CountDown(3):

    print(n) # 输出 3 2 1
```


## 如何重复使用迭代器数据

### 将其转换为列表

data = list(csv_reader)

## 使用一些其他的包
import itertools

iter1, iter2 = itertools.tee(original_iter, 2)

---

> Author: <no value>  
> URL: https://littleju1y.github.io/%E6%9D%82%E4%B8%83%E6%9D%82%E5%85%AB/python%E8%BF%AD%E4%BB%A3%E5%99%A8/  

