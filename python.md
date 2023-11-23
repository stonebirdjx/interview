<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [装饰器](#%E8%A3%85%E9%A5%B0%E5%99%A8)
- [迭代器](#%E8%BF%AD%E4%BB%A3%E5%99%A8)
- [生成器](#%E7%94%9F%E6%88%90%E5%99%A8)
- [lambda 匿名函数及作用](#lambda-%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0%E5%8F%8A%E4%BD%9C%E7%94%A8)
- [赋值、浅拷贝和深拷贝的区别？](#%E8%B5%8B%E5%80%BC%E6%B5%85%E6%8B%B7%E8%B4%9D%E5%92%8C%E6%B7%B1%E6%8B%B7%E8%B4%9D%E7%9A%84%E5%8C%BA%E5%88%AB)
- [超出有效索引范围](#%E8%B6%85%E5%87%BA%E6%9C%89%E6%95%88%E7%B4%A2%E5%BC%95%E8%8C%83%E5%9B%B4)
- [修改list影响循环](#%E4%BF%AE%E6%94%B9list%E5%BD%B1%E5%93%8D%E5%BE%AA%E7%8E%AF)
- [dict遍历过程中插入会报错](#dict%E9%81%8D%E5%8E%86%E8%BF%87%E7%A8%8B%E4%B8%AD%E6%8F%92%E5%85%A5%E4%BC%9A%E6%8A%A5%E9%94%99)
- [不要使用可变对象作为函数默认值](#%E4%B8%8D%E8%A6%81%E4%BD%BF%E7%94%A8%E5%8F%AF%E5%8F%98%E5%AF%B9%E8%B1%A1%E4%BD%9C%E4%B8%BA%E5%87%BD%E6%95%B0%E9%BB%98%E8%AE%A4%E5%80%BC)
- [IndexError – 列表取值超出了他的索引数](#indexerror--%E5%88%97%E8%A1%A8%E5%8F%96%E5%80%BC%E8%B6%85%E5%87%BA%E4%BA%86%E4%BB%96%E7%9A%84%E7%B4%A2%E5%BC%95%E6%95%B0)
- [列表的+和+=, append和extend](#%E5%88%97%E8%A1%A8%E7%9A%84%E5%92%8C-append%E5%92%8Cextend)
- [== 和 is 的区别](#-%E5%92%8C-is-%E7%9A%84%E5%8C%BA%E5%88%AB)
- [bool其实是int的子类](#bool%E5%85%B6%E5%AE%9E%E6%98%AFint%E7%9A%84%E5%AD%90%E7%B1%BB)
- [元组是不是真的不可变?](#%E5%85%83%E7%BB%84%E6%98%AF%E4%B8%8D%E6%98%AF%E7%9C%9F%E7%9A%84%E4%B8%8D%E5%8F%AF%E5%8F%98)
- [强制浮点除法](#%E5%BC%BA%E5%88%B6%E6%B5%AE%E7%82%B9%E9%99%A4%E6%B3%95)
- [Python内存管理](#python%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86)
- [Python的垃圾回收](#python%E7%9A%84%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)
  - [引用计数](#%E5%BC%95%E7%94%A8%E8%AE%A1%E6%95%B0)
  - [标记-清除](#%E6%A0%87%E8%AE%B0-%E6%B8%85%E9%99%A4)
- [内存优化的策略](#%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96%E7%9A%84%E7%AD%96%E7%95%A5)
- [list 扩容](#list-%E6%89%A9%E5%AE%B9)
- [dict 扩容](#dict-%E6%89%A9%E5%AE%B9)
- [什么是闭包？](#%E4%BB%80%E4%B9%88%E6%98%AF%E9%97%AD%E5%8C%85)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 装饰器

语法糖 @

装饰器的本质是一个以函数作为参数的高阶函数，作用是在不改变函数的情况下，添加额外的功能（如计算执行时间、添加日志等）。

**装饰器顺序**

```python
@a
@b
@c
def f ():
    pass
f = a(b(c(f)))
```

**带参数的装饰器**

```python
def use_logging(level):#@use_logging(level="warn")
    pass 
```

**类装饰器**

必须要有`__call__`方法

`__init__` 里面传入func

```python
class myDecorator(object):
     def __init__(self, f):
         print("inside myDecorator.__init__()")
         f() # Prove that function definition has completed
     def __call__(self):
         print("inside myDecorator.__call__()")
 
 @myDecorator
 def aFunction():
     print("inside aFunction()")
```

**wraps**

```python
# functools.wraps，wraps本身也是一个装饰器，它能把原函数的元信息拷贝到装饰器里面的 func 函数中
from functools import wraps
def logged(func):
    @wraps(func) #可以保留原func的属性
    def with_logging(*args, **kwargs):
        print func.__name__      # 输出 原func
        print func.__doc__       # 输出 原func
        return func(*args, **kwargs)
    return with_logging
```

# 迭代器 

迭代器是一个可以记住遍历的位置的对象。
迭代器有两个基本的方法：iter() 和 next() 或者 c.__next__
一个类作为一个迭代器使用需要在类中实现两个方法 __iter__() 与 __next__()

# 生成器

使用了 yield 的函数被称为生成器（generator）。
yield本身就是一个迭代器
yield from iter 
yield 可以接收send发送的数据

# lambda 匿名函数及作用

```python
f1 = lambda x,y:x*x+y*y
f1(1,2)
```

语音简洁，提取重复代码块儿，方便使用与维护

# 赋值、浅拷贝和深拷贝的区别？

- 赋值：对象的赋值就是简单的对象引用
- 浅拷贝：浅拷贝会创建新对象，引用原内容的地址，而是原对象内第一层对象的引用。
- 深拷贝：深拷贝只有一种形式，copy 模块中的 deepcopy() 函数。深拷贝拷贝了对象的所有元素，包括多层嵌套的元素。因此，它的时间和空间开销要高。

# 超出有效索引范围

当start或stop超出上文提到的有效索引范围时，切片操作不会抛出异常，而是进行截断。

```Python
 a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
 >>> a[-100:5]
 [0, 1, 2, 3, 4]
 >>> a[5:100]
 [5, 6, 7, 8, 9]
 >>> a[-100:100]
 [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

# 修改list影响循环

```Python
l = ["123","456","789"]
for i in l :
    l.append(i)
    print(i)
```

# dict遍历过程中插入会报错

```Python
m = {
    "name": "test", 
    "age": 20, 
    "gender": "male"
}

for k,v in m.items():
    print(k,v)
    m['k'+"append"]= v

# RuntimeError: dictionary changed size during iteration
```

# 不要使用可变对象作为函数默认值

Python 解释器执行 def 语句时,在全局命名空间，有一个函数名（变量叫 func）会指向该函数对象，记住，至始至终，不管该函数调用多少次，函数对象只有一个，就是function object，不会因为调用多次而出现多个函数对象。

```Python
def abc(a, b=[]):
    b.append(a)
    print(b)
    return b

if __name__ == '__main__':
    abc(1)
    abc(2)
    abc(3)

[1]
[1, 2]
[1, 2, 3]
```

# IndexError – 列表取值超出了他的索引数

```Python
my_list = [1, 2, 3, 4, 5]
In: my_list[5] # 根本没有这个元素, IndexError
In: my_list[5:] # 但是可以这样,一定要注意, 用好了是trick,用错了就是坑啊
Out: []
```

# 列表的+和+=, append和extend

```Python
myList = [1,2,3,4]

# +返回新列表
myList + [1] # [1, 2, 3, 4, 1]
print(myList) # [1, 2, 3, 4]

# +=
myList += [1]
print(myList) # [1, 2, 3, 4, 1]
```

# == 和 is 的区别

`is`是判断2个对象的身份, `==`是判断2个对象的值.

# bool其实是int的子类

```Python
isinstance(True, int)
# True
```

# 元组是不是真的不可变?

元组中的可变对象，依旧可变

可以重新定义元祖

```Python
tup = ([],)
tup[0].extend([1])


my_tup = (1,)
my_tup += (4,)
my_tup = my_tup + (5,)
print(my_tup) #(1, 4, 5) ? 嗯 不是不能操作元组嘛?
```

# 强制浮点除法

```Python
a = 3
b = 4
result = 1.0 * a / b
```

# Python内存管理

Python的内存管理是自动的。它由Python的内存管理器负责，当你创建一个对象时，Python会自动分配内存给它；当对象不再使用时，Python会自动回收它的内存。

当要分配内存空间时，不单纯使用 malloc/free，而是在其基础上堆放3个独立的分层，有效率地进行分配。

Python的内存管理器实际上是一个内存池。它分为若干个固定大小的内存块，每当有新的对象需要内存时，Python就从内存池中分配一个内存块给它。这种内存管理方式可以避免频繁地向操作系统申请和释放内存，从而提高性能。

# Python的垃圾回收

Python使用引用计数和标记-清除两种方式来进行垃圾回收。

## 引用计数

Python中的每一个对象都有一个引用计数，用来记录有多少引用指向它。当引用计数降为0时，Python就会释放这个对象的内存。

```Python
import sys

s = 'hello, world'
print(sys.getrefcount(s))  # 输出: 2
```

## 标记-清除

尽管引用计数是一种有效的垃圾回收方式，但它不能处理循环引用的情况。例如，如果两个对象相互引用，那么它们的引用计数永远都不会降为0，即使没有其他引用指向它们。

```Python
a = []
b = []
a.append(b)
b.append(a)
```

当Python进行标记-清除时，它会找出这个循环引用，并释放a和b的内存。

# 内存优化的策略

- 尽量使用局部变量: 局部变量的生命周期通常比全局变量短，当函数或方法执行完成后，局部变量就会被销毁，它所占用的内存就会被释放。因此，尽量使用局部变量可以帮助我们节省内存。
- 使用生成器: 可以在每次请求时生成一个新的值，而不是一次性生成所有的值。这样可以大大节省内存。
- 避免循环引用

# list 扩容

- 初始容量：当创建一个空列表时，Python 会为其分配一定的初始容量，通常为 4 或 8 个元素的空间。
- 扩容策略：当列表的元素数量超过当前容量时，Python 会根据一定的扩容策略来增加列表的容量。一般来说，Python 会将当前容量翻倍，并重新分配更大的存储空间。

# dict 扩容

- 初始容量：当创建一个空字典时，Python 会为其分配一定的初始容量，通常为 8 个哈希桶（buckets）的空间。每个哈希桶是一个存储键值对的槽位。
- 负载因子：字典的负载因子是指字典中已占用的槽位数量与总槽位数量的比值。Python 使用一个默认的负载因子阈值（load factor threshold），通常为 0.66，即当字典的负载因子超过 0.66 时触发扩容操作。
- 扩容策略：当字典的负载因子超过阈值时，Python 会自动触发字典的扩容操作。扩容涉及以下步骤：
  - 创建一个新的更大的哈希表，通常是当前哈希表容量的两倍。
  - 将原始哈希表中的键值对重新哈希到新的哈希表中，并存储在新的槽位中。
  - 释放原始哈希表占用的内存空间。
- 哈希冲突解决：在重新哈希过程中，如果发生哈希冲突，Python 使用开放寻址法（open addressing）来解决冲突。具体来说，它会在新的哈希表中查找下一个可用的槽位，直到找到一个空闲的位置来存储键值对。

# 什么是闭包？

闭包是指在一个函数内部定义另一个函数，并且这个内部函数能够访问外部函数的变量和参数，即使外部函数已经执行完毕，这些变量和参数仍然存在于内存中，并可以被内部函数使用。闭包常常用来实现一些高阶函数的功能，例如装饰器、回调函数等。