---
title:  "Python Basic"
date:   2018-03-16 11:01:01 +0800
categories: python
tags: python
---

# Table of Content

<!-- TOC -->

- [Python 基础](#python-基础)
  - [标识符](#标识符)
  - [变量](#变量)
    - [变量作用域](#变量作用域)
  - [数据类型](#数据类型)
  - [控制语句](#控制语句)
  - [运算符](#运算符)
  - [集合](#集合)
  - [表达式](#表达式)
  - [函数](#函数)
  - [面向对象](#面向对象)
  - [module and package](#module-and-package)
- [I/O](#io)
- [References](#references)

# Python 基础

## 标识符

* \_单下划线：弱“内部使用”标识。对于“from M import *”，将不导入所有以下划线开头的对象，包括包、模块、成员。
* 单下划线结尾\_：为了避免与python关键字的命名冲突
* \_\_双下划线开头：模块内的成员，表示私有成员，外部无法直接调用
* \_\_双下划线开头双下划线结尾\_\_：指那些包含在用户无法控制的命名空间中的“魔术”对象或属性，如类成员的name 、doc、init、import、file、等。推荐永远不要将这样的命名方式应用于自己的变量或函数。

## 变量

Python 变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。

例子：

```python
 counter = 100 # 赋值整型变量
 miles = 1000.0 # 浮点型
 name = "John" # 字符串

 print counter
 print miles
 print name
```

多个变量赋值：

* Python允许你同时为多个变量赋值。例如：

```python
a = b = c = 1
```

以上实例，创建一个整型对象，值为1，三个变量被分配到相同的内存空间上。

* 也可以为多个变量指定多个值。例如：

```python
 a, b, c = 1, 2, "john"
```

以上实例，两个整型对象1和2的分配给变量a和b，字符串对象"john"分配给变量c。

### 变量作用域

定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

* 全局变量
  * 如果在函数内声明了同名变量，那么该变量是局部变量。如果要在函数内修改全局变量，需先声明: global [variable]。
* 局部变量

示例：

```python
total = 0; # This is global variable.
def sum( arg1, arg2 ):
    # global total # 去掉本行注释，则声明这里的 total 是全局变量
    total = arg1 + arg2; # 这里的 total 是局部变量.
    print "Inside the function local total : ", total
    return total;

sum( 10, 20 );
print "Outside the function global total : ", total
```

## 数据类型

Python数字类型转换

| 函数                   | 返回值                                              |
|------------------------|:----------------------------------------------------|
| long(x [,base ])       | 将x转换为一个长整数                                 |
| float(x )              | 将x转换到一个浮点数                                 |
| complex(real [,imag ]) | 创建一个复数                                        |
| str(x )                | 将对象 x 转换为字符串                               |
| repr(x )               | 将对象 x 转换为表达式字符串                         |
| eval(str )             | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
| tuple(s )              | 将序列 s 转换为一个元组                             |
| list(s )               | 将序列 s 转换为一个列表                             |
| chr(x )                | 将一个整数转换为一个字符                            |
| unichr(x )             | 将一个整数转换为Unicode字符                         |
| ord(x )                | 将一个字符转换为它的整数值                          |
| hex(x )                | 将一个整数转换为一个十六进制字符串                  |
| oct(x )                | 将一个整数转换为一个八进制字符串                    |

## 控制语句

* if-elif-else
* try-except-finally
* pass 不执行任何操作
* raise 抛出异常
* import 语句
  * from module import name
  * import module as name
  * from module import name as anothername

## 运算符

* 逻辑运算符：and，or，not 表示逻辑与，或，非.
* 成员运算符：in, not in
* 身份运算符：is, is not, 判断两个标识符是不是引用自一个对象

## 集合

* list - [], 列表可修改。
* tuple - (), 元组不能修改(当元组只包含一个元素时，需要在元素后面添加逗号, 如(50,))。
* dict or map - {k1: v1, k2: v2}。
* set - {'Tom', 'Gavin'}，创建空集合必须使用 set() 而不是 {}，因为 {} 是创建空字典。集合支持的运算符(-，|，&，^)。
* list slice - nums[2:5]。

## 表达式

* y if z else x
* [x + 3 for x in range(4)]

## 函数

参数：

* 默认参数
* 可变参数 - *args, 表示这是由多个实参组成的可变参数，该形参视作tuple数据类型;
* 可变参数 - \*\*kwargs, 表示这是由多个实参组成的可变参数，该形参视作dict数据类型;

__Note:__

>函数的缺省参数值在连续多次调用该函数时，如果不被实参值覆盖，就会一直保留。例如：

```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
```

>结果为：
```
[1]
[1, 2]
[1, 2, 3]
```

## 面向对象

定义：

```python
class Fish(object)
    def eat(self, food):
        print("eating %s" % food);
```

self 是约定俗成的写法，也可以使用如 myself。

使用：

```python
#构造Fish的实例：
f=Fish()
#以下两种调用形式是等价的：
Fish.eat(f, "earthworm")
f.eat("earthworm")
```

## module and package

* module(模块)：
> 模块就是一个保存了Python代码的文件。文件名去掉.py 后缀就是模块名。  
> 在模块内通过 global variable __name__ 访问模块名。

* package(包):
> 包是一个分层次的文件目录结构。  
> 通过目录中包含 \_\_init\_\_.py 文件，将包与普通目录区分开来的。

# I/O

* File 对象: file 对象提供了操作文件内容的一系列方法。
* OS 对象: 提供了处理文件及目录的一系列方法。

# References

* [Wikipedia - Python](https://zh.wikipedia.org/wiki/Python)
* [W3Cschool - Python](https://www.w3cschool.cn/python)
* [Python 3 教程](https://www.w3cschool.cn/python3/)