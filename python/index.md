# Python学习笔记

# Python学习笔记

## 1.python基础

### 1.1python语法常识

> * **python语法采用缩进格式而不是括号来进行代码块分割的**
> * **缩进没有空格个数或者`tap`键的约束，但应坚持使用*四个空格*缩进**
> * **当语句以`:`结尾时，缩进的语句视为代码块**
> * **以#开头的是注释**
> * **复制粘贴功能基本等于失效，粘贴的代码必须检查缩进是否正确**
> * **python是大小写敏感的，写错了大小写，程序会报错**
>
> > ```python
> > #print
> > a = 100
> > if a >= 0:
> > 	print(a)
> > else:
> > 	print(-a)
> > ```

### 1.2基本数据类型

******

#### 1.2.1数据类型分类

##### 1.2.1.1数字型与非数字型

- `python`中的变量一般分为两种类型 **数字型**和**非数字型**
- **数字型**
  - **整型**
  - **浮点型**
  - **布尔型**
  - **复数型**
- **非数字型**
  - **字符串**
  - **列表（list）**
  - **元组(tuple)**
  - **字典(dict)**

##### 1.2.1.2不可变类型与可变类型

> `python`中变量都是通过对数据的引用来定义的。
>
> 不可变类型只能在内存中重建一组数据，然后让变量指向数据
>
> 可变类型直接可以改内存中定义好的数据
>
> 给变量赋新值是改变变量指向的地址
>
> 通过方法改变数据的值不影响变量指向的地址

* **不可变类型**
  * 数字类型
  * 字符串
  * 元组
* **可变类型**
  * 列表
  * 字典
* **哈希函数**`hash`
  * 接收一个**不可变类型**作为参数
  * 返回值是一个**整数**
* `python`中设置字典**键值对**时，会首先对`key`进行`hash()`决定数据如何在内存中保存，方便之后的**增删改查**
  * 键值对的`key`必须是不可变类型
  * 键值对的`value`可以是任何类型

#### 1.2.2基本数据类型介绍

****

* `python`是**动态语言**变量本身**类型不固定**

* **整数**

  * 十六进制前面用**`0x`**例`0xff00`
  * `python`的整数和浮点数**没有大小限制**
  * `/`结果是精确的，`//`是除法结果取整，`%`是取余

* **浮点数**

  * `1.23*10^9`等于`1.23e9`
  * 浮点数有**误差**

* **字符串**

  * 用`''`或`""`扩起来的任意文本

  * 转义字符`\`

  * 可以用`r''`表示`''`内部的字符串默认不转义

  * 字符串内部有多个换行，写在`\`中不好阅读`'''...'''`表示多行内容

    * `...`是提示符，不是代码的一部分，**不需要手打**

    ```python
    print('''line1
    ...line2
    ...line3''')
    line1
    line2
    line3
    ```

    ```python
    print(r'''hello,\n
    world''')
    hello,\n
    world
    ```

* **布尔**

  * `True False `
  * `and or not`运算
  * `3>2`输出`True`

* **空值**

  * `None`表示空值

* **常量**

* **变量**

  * `python`是**动态语言**
  * 首先在内存中创建值，然后将变量指向该值
  * `x = y`是把变量`x`指向真正的对象，`y`改变**不影响**`x`的值

### 1.3运算符

****

#### 1.3.1算术运算符

| 运算符 | 描述   |
| ------ | ------ |
| **     | 乘幂   |
| +      | 加法   |
| -      | 减法   |
| /      | 除法   |
| *      | 乘法   |
| //     | 取整数 |
| %      | 取余   |

> `*`还可用于字符串，结果是将运算符重复n次
>
> 运算符优先级与C语言类似。可以通过`()`来增加优先级

#### 1.3.2赋值运算符

****

| 运算符 | 描述       | 实例                |
| ------ | ---------- | ------------------- |
| =      | 赋值       | c = a               |
| +=     | 加法赋值   | c += a c = c + a    |
| -=     | 减法赋值   | c -= a c = c - a    |
| /=     | 除法赋值   | c /= a c = c / a    |
| *=     | 乘法赋值   | c *= a c = c * a    |
| //=    | 取整数赋值 | c //=a c = c //a    |
| %=     | 取余赋值   | c %= a c = c % a    |
| **=    | 幂赋值     | c **= a  c = c ** a |

#### 1.3.3比较运算符

| 运算符 | 描述         | 举例 |
| ------ | ------------ | ---- |
| ==     | 等于判断     | a==b |
| !=     | 不等于判断   | a!=b |
| >      | 大于判断     | a>b  |
| <      | 小于判断     | a<b  |
| >=     | 大于等于判断 | a>=b |
| <=     | 小于等于判断 | a<=b |

> 结果为True和False

#### 1.3.4逻辑运算符

| 运算符 | 描述                                       | 举例    |
| ------ | ------------------------------------------ | ------- |
| and    | if *x* is false, then *y*, else *x*        | a and b |
| or     | if *x* is false, then *x*, else *y*        | a or b  |
| not    | if *x* is false, then `True`, else `False` | not a   |

#### 1.3.5位运算符

| 运算符 | 描述     | 举例   |
| ------ | -------- | ------ |
| \|     | 按位与   | a \| b |
| ^      | 按位异或 | a ^ b  |
| &      | 按位与   | a&b    |
| <<     | 左移     | a<<b   |
| >>     | 右移     | a>>b   |
| ~      | 按位取反 | ~a     |



### 1.4字符串和编码

******

#### 1.4.1字符串和编码总览

* **ASCII编码**是**一个字节**，表示数量有限
* **Unicode编码**是**两个字节**,但是空间多一倍
* **UTF-8可变长**，把一个Unicode字符编码成1-6个字节。英文是一个字节，汉字通常三个字节，大量英文就能升空间。

> * **本地计算机**中**存储用UTF-8**，**内存中用Unicode**，需要时**互相转换**
> * **浏览网页**,**服务器Unicode**,转换成**UTF-8**到**网页**

#### 1.4.2python的字符串

******

**python字符串是以`Unicode`来编码的**

*****

* **`ord('A')`**可获取字符的**整数**表示

```python
>>> ord('A')
65
>>> ord('中')
20013
```

* **`chr(66)`**把整数表示转换为对应的字符

```python
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

* **`.encode()`** 将字符串用**特定编码方式**转换为**字节**输出。
* **`.decode`**将字节读入的字符串按特定编码方式输出

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'

>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'.encode('ascii') 

#报错
>>> '中文'.encode('ascii')
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')

#忽视少数字符
>>> b'\xe4\xb8\xad\xff'.decode('utf-8',errors='ignore')
'中'
```

* **`len()`**计算**字符串**的**字符数**,如果是**字节表示**的，就计算**字节数**

```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
>>> len('ABC')
3
>>> len('中文')
2
```

* python文本文件**储存和读取格式**

  * 指定python**读取**格式

  ```python
  #！/usr/bin/env python3
  # -*- coding: utf-8 -*-
  ```

  * 指定python**储存**格式

  > **储存格式需要通过编辑器指定**

* python字符串格式化的第一种形式

  * 与C语言一致，通过`%d%f%s%x`指定

  ```python
  >>> 'hello,%s' % 'world'
  'hello,world'
  >>> 'Hi,%s,you have $%d'%('Michael',1000)
  'Hi,Michael,you have $1000'
  ```

  * 不知道应该用什么,`%s`永远起作用

* python字符串格式化的第二种形式

  ```python
  >>> 'Hello,{0},成绩提升了 {1:.1f}%'.format('小明',17.125)
  'Hello,小明，成绩提升了 17.1'
  
  >>> str = 'Hello,{0},成绩提升了 {1:.1f}%'
  >>> print(str.format('小明',17.125))
  'Hello,小明，成绩提升了 17.1'
  ```

### 1.5条件判断语句

****

```python
if salary >= 10000:
	print('happy')
elif salary >= 5000:
	print('common')
else:
	print('sad')
```

### 1.6循环语句

****

```python
#for循环
sum = 0
for x in [1,2,3,4,5,6,7,8,9]:
	sum = sum + x
print(sum)

#range函数生成证书序列，和list()函数转换为list
>>> list(range(5))
[0,1,2,3,4]

#举例
sum = 0
for x in range[101]:
	sum = sum + x
print(sum)
else:
	没有通过nreak跳出循环，循环结束后会执行的代码
5050

#while循环
sum = 0
n = 99
while n > 0 :
    sum = sum + n
    n = n-2
print(sum)

##break和continue两函数与C语言用法一样
```

### 1.7高级数据类型

****

#### 1.7.1变量的分类与简介

##### 1.7.1.1数字型与非数字型

* `python`中的变量一般分为两种类型 **数字型**和**非数字型**
* **数字型**
  * **整型**
  * **浮点型**
  * **布尔型**
  * **复数型**
* **非数字型**
  * **字符串**
  * **列表（list）**
  * **元组(tuple)**
  * **字典(dict)**

* 非数字变量的**特点**
  * 都可以看成一个**序列（`sequence`)**,也可以理解为**容器**
  * **取值`[]`**
  * **遍历`for x in`**
  * **计算长度、大小，最大小值、比较，删除**
  * **链接`+`和重复`*`**
  * **切片**

##### 1.7.1.2可变类型与不可变类型

> `python`中变量都是通过对数据的引用来定义的。
>
> 不可变类型只能在内存中重建一组数据，然后让变量指向数据
>
> 可变类型直接可以改内存中定义好的数据
>
> 给变量赋新值是改变变量指向的地址
>
> 通过方法改变数据的值不影响变量指向的地址

- **不可变类型**
  - 数字类型
  - 字符串
  - 元组
- **可变类型**
  - 列表
  - 字典
- **哈希函数**`hash`
  - 接收一个**不可变类型**作为参数
  - 返回值是一个**整数**
- `python`中设置字典**键值对**时，会首先对`key`进行`hash()`决定数据如何在内存中保存，方便之后的**增删改查**
  - 键值对的`key`必须是不可变类型
  - 键值对的`value`可以是任何类型

##### 1.7.1.3局部变量与全局变量

* **局部变量**在**函数内部**定义，只在*该函数内部使用**
* **全局变量**在**函数外部**定义,**所有函数**都可以使用该变量
* 大多数语言**不推荐使用全局变量**
* 函数在使用变量时，会首先检索局部变量，如果没有局部变量，会检索全局变量

###### 1.7.1.3.1局部变量

* **局部变量**是在**函数内部定义**的变量，只有在**该函数内部**才可以使用
* **函数执行结束**后，函数内部的**局部变量会被系统回收**
* 不同函数中**局部变量可以重名**，且无影响
* 局部变量的**生命周期**
  * **生命周期**指从**被创建到被系统回收**的过程
  * 局部变量在**函数执行时才被创建**，函数**执行结束后被回收**
  * 局部变量在生命周期内，可以用来**保存函数内部使用到的数据**

###### 1.7.1.3.2全局变量

* 全局变量是在**函数外部定义**的变量，所有函数都可以使用
* 默认函数内部**不允许直接修改全局变量的引用,使用赋值语句修改全局变量的值**
* 如果使用使用赋值语句修改全局变量的值，会重新定义一个局部变量

* 要在函数内部修改全局变量的名，需要加`global`关键字声明变量

* 应将全局变量定义在所有函数的上方

  ```python
  num = 10
  def demo1():
  	global num
  	num = 20
  #这样可以修改全局变量
  #global关键字会告诉解释器，后面的值是一个全局变量，
  #再使用赋值语句时，不会再创建局部变量
  ```

  

#### 1.7.2集合类型`list(列表)`和`tuple（元组）`

****

##### 1.7.2.1`list`(类似于空类型集合)

* 列表支持的方法

```python
#取值,索引超出范围会报错
list[0]
#取索引，数据不在列表内会报错
list.index('')
#修改数据,索引超出范围会报错
list[1] = '李四'
#增加数据，append会插到最后,insert会插到指定索引前面,extend可以把一个列表追加到当前列表末尾
#列表中使用+=运算符会直接调用列表的extend()方法
#相加再赋值,并不会调用extend()方法
list.append('李四')
list.insert(1,'李四')
list.extend(name_list)
#删除数据，remove会删除第一个出现的元素，元素不存在时，会报错。clear会把列表清空，pop默认把列表最后一个元素删除，同时可以指定删除元素的索引.建议使用前三个方法。del关键字本质上是将一个变量从内存中删除
list.remove('李四')
list.pop(3)
list.clear()
del list[0]
#统计,len统计列表元素个数，count统计某个元素出现的个数
len(list)
list.count('李四')
#列表排序，sort升序排列，sort(reverse=True),list.reverse()降序排列
list.sort()
list.sort(reverse=True)
list.reverse()
```



- `list`定义方法(类似于可变数组)

```python
classmates = ['a','b','c']
```

- `list`访问方式（与数组类似）

```python
#第一种，正序访问
classmates[0]
classmates[1]
classmates[2]
#第二种，逆序访问
classmates[0] = classmates[-3]
classmates[2] = classmates[-1]
```

- `list`追加元素到末尾

```python
>>> classmates.append('d')
>>> classmates
['a','b','c','d']
```

- `list`把元素插入特定位置

```python
>>> classmates.insert(1,'d')
>>> classmates
['a','d','b','c','d']
```

- `list`删除元素

```python
>>> classmates.pop()
'd'
>>> classmates
['a','d','b','c']
>>> classmates.pop(1)
'd'
>>> classmates
['a','b','c']
```

- `list`改变元素

```python
>>> classmates[1] = 'd'
>>> classmates
['a','d','c']
```

- `list`元素类型可以是任意的，也可以不同

```python
>>> L = ['a',123,True]
>>> s = ['a','b',['c','d'],'d']
>>> len(s)
4
#等同于以下
>>> p = ['c','d']
>>> s = ['a','b',p,'d']
>>> len(s)
4
#预访问C，可采用一下两种形式
>>> p[0]
>>> s[2][0]
#空list
>>> L = []
>>> len(L)
0
```

* `list`使用场景
  * 将多个同类型的元素保存到`list`里，遍历处理

> **`list`类似于`Java`的空类型集合`list`**
>
> **len()函数可以获得`list`元素的个数**

##### 1.7.2.2`tuple`(类似与数组，一旦初始化就不能改变)

------

* `tuple`支持的方法

```python
#获取索引
info.index('李四')
#获取某数据出现的次数
info.count('李四')
#统计元素格式
len(info)
```

- `tuple`定义只有一个元素的该类型时，需加一个`,`

```python
>>> t = (1,)
>>> t
(1,)
#否则会产生这样的歧义
>>> t = (1)
>>> t
1
#定义空元组
t = ()
```

- `tuple`虽然不可变，但是包含的`List`可以变，因为包含的`list`是以指针指向的形式保存的,`list`是可变的

```python
>>> t = (0,1,['A','B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
(0,1,['X','Y'])
```

* 元组使用场景

  * 函数**多个参数和多个返回值**，以**元组**实现
  * 格式字符串（%d,%s），以**元组**实现

  ```python
  print('%s, %d' % ('李四'，14))
  #等于
  info_tuple = ('李四'，14)
  print('%s，%d' % info_tuple)
  #等于
  info_str = '%s,%d' % info_tuple
  print(info_str)
  ```

  * **让列表数据不被修改**`tuple(list)`

* **修改`tuple`，可`list(tuple)`，修改之后的`list`，然后再`tuple(list)`**

#### 1.7.3使用`dict`和`set`

****

##### 1.7.3.1`dict`

****

* `key`只能使用**字符串，数字或者元组**
* 字典的常用操作

```python
#取值,如果key值不存在，则会报错
person_dict['key']
#增加'key'不存在，则增加
persion_dict['key'] = 18
#修改，'key'存在，则修改
persion_dict['key'] = 20
#删除
persion_dict.pop('key')
#统计键值对的数量
len(person_dict)
#合并字典,被合并的字典中包含已经存在的键值对，会覆盖原有的键值对
person_dict.update(dict)
#清空
persion_dict.clear()
#循环遍历
#k是每一次循环中，获取的键值对的key
for k in persion_dict :
    print('%s - %s' % (k,persion_dict[k]))
```



* `dict`类似与`Java`中的`map`

  > 通过key来映射value,所以key的值只能是**不可变量**，**不能是list类型**
>
  > 字典无序，列表有序

  * 创建和使用

  ```python
  >>> d = {'a' : 95,'b' : 75,'c' : 85}
  >>> d['a']
95
  ```

  * 放入数据

  ```python
  >>> d['d'] = 67
  >>> d['d']
67
  ```

  * 改变数据

  ```python
  >>> d['a'] = 90
  >>> d['a']
90
  ```

  * 查询是否存在

  ```python
  #方法一
  >>> 'e' in d
  False
  #方法二,如果不存在返回none或者指定的值，none在交互环境下不显示结果
  >>> d.get('e')
  >>> d.get('e',-1)
-1
  ```

  * 删除数据 

  ```python
  >>> d.pop('a')
  90
  >>> d
{'b' : 75,'c' : 85,'d' : 67}
  ```

* 使用场景
  * 将多个字典放在一个列表里，循环遍历处理

##### 1.7.3.2`set`

****

* `set`类似与`java`中的`set`

  * 创建（需要通过`list`创建)

  ```python
  >>> s = set ([1,2,3,2,3])
  >>> s
  {1,2,3}
  ```

  * 添加

  ``` python
  s.add(4)
  ```

  * 删除

  ```python
  s.remove(4)
  ```

  * 交并

  ```python
  >>> s1 = set([1,2,3])
  >>> s2 = set([2,3,4])
  >>> s1 & s2
  {2,3}
  >>> s1 | s2
  {1,2,3,4}
  ```

#### 1.7.4字符串

* 因为大部分变成语言字符串都是双引号，尽量使用`双引号`定义字符串
* 可以使用`for`循环来访问字符串的每个字符

##### 字符串的常用操作,可以查文档

```python
#统计字符串长度
len(str)
#统计某子字符串出现的次数
str.count("llo")
#某一个子字符串出现的位置,该子字符串不存在时会报错
str.index("llo")
```

#### 1.7.5高级数据类型公共方法

##### 1.7.5.1公共函数

| 函数   | 描述                 | 备注                    |
| ------ | -------------------- | ----------------------- |
| len(a) | 计算容器中元素的个数 |                         |
| del(a) | 删除变量             | 注意与关键字区分        |
| max(a) | 返回容器中的最大值   | 如果是字典，仅针对key值 |
| min(a) | 返回容器的最小值     | 如果是字典，仅针对Key值 |

##### 1.7.5.2切片(字符串，元组，列表支持，字典不支持)

* 切片使用索引值来从较大容器中切出小容器。

##### 1.7.5.3运算符

> 列表中使用`+=`运算符会直接调用列表的`extend()`方法
>
> 列表中先相加再赋值，并**不会**调用`extend()`方法

|    运算符    | 举例            | 结果                  | 描述           | 支持的数据类型                                |
| :----------: | --------------- | --------------------- | -------------- | --------------------------------------------- |
|      +       | [1,2]+[3,4]     | [1,2,3,4]             | 合并           | 字符串，元组，列表(生成一个新的)              |
|      *       | ["Hi"]*4        | ["Hi","Hi","Hi","Hi"] | 重复           | 字符串，元组，列表                            |
|      in      | 3 in(1,2,3)     | True                  | 元素是否存在   | 字符串，元组，列表，字典(字典中，必须要用key) |
|    not in    | 3 not in(1,2,3) | False                 | 元素是否不存在 | 字符串，元组，列表，字典                      |
| >,>=,<,<=,== | (1,2,3)<(2,2,3) | True                  | 元素比较       | 字符串，元组，列表                            |

* 成员运算符
  * `in`和`not in`

## 2.函数

* **定义函数**

```python
#普通定义
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
    
#空语句,可以作为占位符
pass


#空函数
def empty():
    pass

#当传入参数个数不对时，会报错
#手动参数检查
def my_abs(x):
    if not isinstance(x,(int,float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
    
#返回多个值
import math
def mov(x,y):
    nx = x + 1
    ny = y + 1
    return nx,ny
#函数的返回值实际上是tuple类型

```

* **函数参数**

> 在python中类型属于对象，变量没有对象
>
> > `a = [1,2,3]`中,`[1,2,3]`是List类型，而`a`没有类型
> >
> > `a`仅仅是对象的引用(一个指针)
>
> 在python中类型分为**可变**类型和**不可变**类型
>
> > * 可变类型：list,dict等
> > * 不可变类型：None,Strings,numbers,tuples等
>
> 不可变类型赋新值时是创建一个新对象，然后抛弃原来的对象
>
> > a = 10然后a = 5，实际上是新创建一个对象5，让a指向5，丢掉原来的10
>
> 可变类型直接更改对象
>
> > la = [1,2,3]然后la[2] = 4，实际上是对象[1,2,3]变成[1,2,4]

```python
#位置参数
def power(x):
    return x*x
#调用 power(3)

def power(x,n):
    s = 1
    while n > 0:
        n = n-1
        s = s * n
    return s
#调用 power(3,4)

#默认参数
#必选参数在前，默认参数在后，否则会报错,多个参数时，将变化大的放前面，变化小的放后面
#默认参数没有按默认顺序输入时，需要把参数名写上
def power(x,n=2)
	s = 1
    while n > 0:
        n = n - 1
        s = s * n
    return s
#调用 power(3)或power(3,4)

def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
#调用 enroll('Sarah', 'F') 或  enroll('Sarah', 'F'，7)或 enroll('Sarah', 'F'，city = 'TianJin')

#可变参数(可参照C语言的指针)
def changeme( mylist ):
   "修改传入的列表"
   mylist.append([1,2,3,4]);
   print("函数内取值: ", mylist)
   return
mylist = [10,20,30];
changeme( mylist );
print("函数外取值: ", mylist)
#结果 函数内取值:  [10, 20, 30, [1, 2, 3, 4]]，函数外取值:  [10, 20, 30, [1, 2, 3, 4]]
#将其中可变参数变为不可变参数
def changeme( mylist = None):
   "修改传入的列表"
   mylist.append([1,2,3,4]);
   print("函数内取值: ", mylist)
   return
mylist = [10,20,30];
changeme( mylist );
print("函数外取值: ", mylist)
#结果 函数内取值:  [10, 20, 30, [1, 2, 3, 4]]，函数外取值:  [10, 20, 30]
#可变参数使用
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
#此方式必须传入list或者dict
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
#此方式可直接如下调用calc(1,2,3,4) 如果有num =  [1,2,3]，那么直接调用calc(*num)


#关键字参数(用于传递dict，传入时可以省略)
#传入任意个数时
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw）
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
#限制关键字参数名字
def person(name, age, *, city, job):
    print(name, age, city, job)
```

### 2.1函数参数和返回值

****

函数根据**有没有参数和返回值有四种分类**

1. 有参数，有返回值
2. 无参数，有返回值
3. 有参数，无返回值
4. 无参数，无返回值

> 1.处理外部数据，加参数
>
> 2.需要向外部汇报结果，返回数据，加返回值

#### 2.1.1函数的返回值

* 仅返回一个返回值`return a` `result = demo()`

* 返回多个返回值`return a1,a2`

  *  方式一`result =  demo()`,`result`是`tuple元组`类型,单独取值需`result[0]`
  * 方式二`b1,b2 = demo()`,此时`b1 = a1`,`b2 = a2`

  > 使用多个变量接收返回值时，变量的个数应该与返回值个数一样
  >
  > 否则会报错

> 扩展，交换两个变量
>
> ```python
> c = a
> a = b 
> b = c
> ```
>
> ```
> 
> a = a + b
> b = a - b
> a = a - b
> ```
> ```python
> #仅python可用的方法
> a,b = b,a
> ```

#### 2.1.2函数的参数

* 函数参数本质上是在函数内部建立的不能全局化的局部变量(数据引用)

* 可变参数和不可变参数

  * 如果在函数内部，在函数内部对变量重新赋值，会创建一个新的局部变量(注意与全局变量和局部变量区分开,函数参数与全局变量一般名字不同，如果名字相同，使用`global`也会报错)
  * 如果在函数内部，对可变类型调用方法改变可变类型的值，会影响数据本来(外部）的值

  > 列表的`+=`是调用`extend()`方法
  >
  > 列表的先相加再赋值，不是调用`extend()`方法

##### 2.1.2.1默认参数(缺省参数)

* 给某个**参数指定一个默认值**,具有默认值的参数叫做**缺省参数**
* 调用时**没有传入缺省参数**的值时，会**使用默认值**
* 使用默认参数（缺省参数），可以简化函数,例如`sort()`函数
* 需要使用**最常见的值作为默认值**
* 如果**值不能确定**，则**不能使用默认参数**

> **注意事项**
>
> > * **默认参数应放在所有参数的最后面，即，在默认参数后面，不允许出现未设置默认值的参数，否则会报错**
> > * **默认参数没有按默认顺序输入时，需要把参数名写上,例**
> >
> > ```python
> > def enroll(name, gender, age=6, city='Beijing')
> > 调用enroll('Sarah', 'F'，city = 'TianJin')
> > 或enroll('Sarah', 'F'，7)
> > ```

##### 2.1.2.2多值参数(可变参数)

* 有两种情况

  * 参数前加`*`，可以传递元组,或者列表,**函数将传进去的元组和列表统一变成元组处理**
  * `元组列表拆包`，将一个元组或列表变成一组数据，在元组或列表前加`*`

  ```python
  def calc(*numbers):
      sum = 0
      for n in numbers:
          sum = sum + n * n
      return sum
  #此方式可直接如下调用calc(1,2,3,4) 如果有num =  [1,2,3]，那么直接调用calc(*num)
  ```

  * 参数前加`**`，可以传递字典
  * `字典拆包`，将`一组字典`变成一组数据，在字典前加`**`

  ```python
  def person(name, age, **kw):
      print('name:', name, 'age:', age, 'other:', kw）
  person('Bob', 35, city='Beijing')
  #结果name: Bob age: 35 other: {'city': 'Beijing'}
  person('Adam', 45, gender='M', job='Engineer')
  #结果name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
  extra = {'city': 'Beijing', 'job': 'Engineer'}
  person('Jack', 24, **extra)
  #结果name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
  ```

  

### 2.2函数递归

```python
#类似于C语言
```

## 3.高级特性

### 切片

* **适用对象**：`list`,`tuple`
* **用法**

```python
 L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
 L[0:3] = L[:3] = ['Michael', 'Sarah', 'Tracy']
 L[-2:] = ['Bob', 'Jack']
 
 L = list(range(100))
 L[:10:2] = [0, 2, 4, 6, 8]
 L[::5] = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```

### 迭代

******

```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print(key)
...
a
c
b

#如果要迭代value，可以用for value in d.values()，如果要同时迭代key和value，可以用for k, v in d.items()

>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
#Python内置的enumerate函数可以把一个list变成索引-元素对
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9
```

### 列表生成式

****

* **运用列表生成式，可以快速生成list，可以通过一个list推导出另一个list，而代码却十分简洁。**

```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]

>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```

### 生成器

****

* **用来保存一种算法**
* **获取下一个值使用`next(g)`**

```python
#普通用法(注意与生成式区分)
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
36
49
64
81
#通过函数来构建生成器
#在执行过程中，遇到yield就中断，下次又继续执行。
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'

>>> f = fib(6)
>>> f
<generator object fib at 0x104feaaa0>
```



### 迭代器

****

* **凡是可作用于`for`循环的对象都是`Iterable`类型；**
* **凡是可作用于`next()`函数的对象都是`Iterator`类型，它们表示一个惰性计算的序列；**
* **集合数据类型如`list`、`dict`、`str`等是`Iterable`但不是`Iterator`，不过可以通过`iter()`函数获得一个`Iterator`对象。**
* **可以通过`list(Iterrator)`此种方式得到一个`Interable`**
* **Python的`for`循环本质上就是通过不断调用`next()`函数实现的.**

```python
for x in [1, 2, 3, 4, 5]:
    pass
```

**等价于**

```python
 首先获得Iterator对象:
it = iter([1, 2, 3, 4, 5])
# 循环:
while True:
    try:
        # 获得下一个值:
        x = next(it)
    except StopIteration:
        # 遇到StopIteration就退出循环
        break
```

## 4.函数式编程

> 特点：允许把函数本身作为参数传入另一函数,还允许返回一个函数

* `python`中变量可以指向函数
* `python`中函数名也是变量

### 高阶函数

* **可以把函数作为参数传入的函数称为高阶函数**

```python
def add(x,y,f):
    return f(x) + f(y)
#调用
>>> print(add(-5,6,abs))
11
```

* `map(f,list)`函数把`list`中的每个数都按照`f`的运算规则计算，得出一个`Iterator`

```python
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

* `reduce(f,list)`函数把`list`中前两个元素按`f`计算，再将结果继续和序列的下一个元素进行累计计算

```python
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```

* `filter(f,list)`函数函数把`list`中的每个数都按照`f`的运算规则计算，如果运算结果是`True`则保留该元素，否则丢弃该元素。

```python
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

* `sorted()`可以对`list`排序，同时也是高阶参数，可以接收一个参数`key`来实现自定义排序
* `key`作用于每个数据，然后根据返回的`list`进行排序

> 默认情况下，对字符串排序，是按照ASCII的大小比较的

```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```

## 5.模块

* **模块需要用`import`关键字导入**
* **每一个以`.py`为结尾的文件都是一个`python模块`**
* **模块中定义的`函数`和`全局变量`都是一个模块中可以提供给外界直接使用的工具**

```python
import a
a.printa()
a.name
```

* 模块也是一个标识符
  * 标识符由**字母、下划线和数字**组成
  * **标识符不能以数字开头**
  * **不能和关键字重名**

* 模块作用域
  * 类似`__xxx__`这样的变量是**特殊变量**，可以被直接引用，但是有特殊用途
  * 似`_xxx`和`__xxx`这样的函数或变量就是**非公开的（private）**，不应该被直接引用

### 模块规范

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'Michael Liao'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```

* 第1行和第2行是标准注释，第1行注释可以让这个`hello.py`文件直接在Unix/Linux/Mac上运行，第2行注释表示.py文件本身使用标准UTF-8编码；

* 第4行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释；

* 第6行使用`__author__`变量把作者写进去
* 模块是一个包含Python定义和语句的文件。文件名就是模块名后跟文件后缀 `.py` 。在一个模块内部，模块名（作为一个字符串）可以通过全局变量 `__name__` 的值获得。

## 6.面向对象编程

### 6.1面向对象与面向过程的区别

* 面向对象是对面向过程进一步的封装，不仅封装了相同的方法，还将相同属性封装在了一起

### 6.2类与对象

****

#### 6.2.1类与对象的概念

****





#### 6.2.2类的设计

****

* 类的命名满足**大驼峰原则**
* **属性**
* **方法**

> 参考面向对象编程的概念

### 6.3面向对象的基础语法

****

#### 6.3.1`dir`内置函数

* `python`中，**变量，数据，函数**都是对象

在`python`中可用两种方法验证：

1. 在**标识符/数据**后面输入一个`.`，按下`tab`键,会提示**方法列表**
2. 使用内置函数`dir`传入**标识符/数据**,可以查看对象内的**所有属性和方法** `def demo()`，`dir(demo)`

> **提示**，`__方法名__`格式的方法是`python`提供的**内置方法/属性**

### 6.3.2定义简单的类

#### 6.3.2.1定义只包含方法的类

* `python`中语法格式如下

```python
class 类名:
    def 方法一(self,参数):
        pass
    def 方法二(self,参数):
        pass
```

* 方法与函数几乎一样
* 方法中的特殊处在于,第一个参数必须是`self`

