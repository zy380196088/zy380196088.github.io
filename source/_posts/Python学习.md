---
title: Python学习
categories: 学习
date: 2017-03-14 18:08:26
tags:
---

先开个坑,记录 Python的学习, 因为我作为一枚小散股民有个量化投资梦...

<!--more-->

# Python 基础

## print()

打印是 Python 中最常用的功能.

## 字符串

"双引号中的字符串",
'单引号和双引号一样',
'''三个引号被用于过于长段的文字或者是说明,只要三个引号不完就可以随意换行写下文字'''

### 字符串的分片与索引 string[x]

字符串的分片是实际上就是从字符串中找出来你要截取的东西,复制出来一小段你要的长度,储存在另一个地方,而不会对原字符串改动.
看完下面的代码,你会对此有进一步的了解:
```python
name = 'My name is Zoey'
print(name[0])
'M'
print(name[-4])
'Z'
print(name[11:14])
'Zoe'
print(name[11:15])
'Zoey'
print(name[5:])
'me is Zoey'
print(name[:5])
'My na'
```

**":"号**两边分别代表字符串的分割从哪里开始哪里结束.
***正数从0开始,表示从前到后;***
***负数从-1开始,表示从后到前;***
**注意,结束的位置,是不截取它本身的(不知道这么说懂不懂,不懂就多看几遍上面的代码吧^-^)**

下面看段有意思的:

```python
//找出你朋友中的魔鬼
word = 'friend'
find_the_evil = word[0] + word[2:4]+word[-3:-1]
print(find_the_evil)
//fiend
```

### type()

通过 type()函数来查看类型.例如:``print(type(word))``

### replace()

```Python
phone_number = '136-6760-2008'
hideing_number = phone_number.replace(phone_number[:9,'*'*9])
print(hideing_number)
//*********2008
```

## 函数

Python3.5包含了68个内键函数,就不一一详细列出了,自行查阅学习吧.

> print()
放入对象就能打印的函数
> input()
让用户输入信息的函数
> len()
测量对象长度
> int()
将字符串类型的数字转换成整数类型的函数

### 创建函数

#### **def functionName(arg1,arg2): return 'something'**

创建函数.

### 传递参数与参数类型

#### 位置参数 (positional argument)

#### 关键词参数 (keyword argument)

在函数调用的时候,将没个参数名称后面赋予一个我们想要传入的值.

#### 给参数设定默认值

定义参数的时候给参数赋值即可.

```python
def sum(num1 , num2, num3 = 100)
    return num1 + num2 + num3
//调用的时候只需要传入 前面两个参数就能正常运行
sum(50,20)
//如果想改变默认参数的值允许函数
sum(50,20,num3 = 50)
```

### open(url,method)

两个参数分别对应文件的完整路径和名称,打开的方式;

### write()

```python
file = open('/Users/zhouyu/Desktop/stack.txt','w')
file.write('Hello World')
```

创建个函数:

```python
def text_create(name,text):
    desktop_path = '/Users/zhouyu/Desktop/'
    full_path = desktop_path + name + '.txt'
    file = open(full_path, 'w')
    file.write(text)
    file.close()
    print('Done')
//调用函数
text_create('hello','Hello World')
```

## 循环与判断

### 布尔类型 True 与 False (注意首字母大写)

但凡能够产生一个布尔值的表达式为布尔表达式(Boolean Expressions)

```python
1>2                 //False
1<2<3               //True
42 != '42'          //True
'Name' == 'name'    //False
```

### 比较运算符

**== , != , > , < , <= , >=**
这些符号的解释一般都能懂就不一一说明了.
**要注意: Python 中有着严格的大小写区分**
**不同类型的对象不能使用"<,>,<=,>="进行比较,却可以使用"==,!=="**

```python
42 > 'number'   //无法比较
42 == 'number'  //False
42 != 'number'  //True
```

***注意:浮点和整数虽然不是同类型,但是不影响比较运算***

```python
5.0 ==5 //True
3.0 >1 //True
```

### 成员运算符 in / not in 与 身份运算符 is / is not

### 布尔运算符 not , and , or

### 条件控制

```python
if condition:
    do something
elif condition:
    do something
else:
    do something
```

### 循环 Loop

#### for 循环

```python
for item in iterable:
    do something
```

#### 嵌套循环

```
for i in iterable:
    for j in iterable:
        do something
```

#### while 循环

```python
while condition:
    do something
```

## 数据结构 Data Structure

### 列表 list = [val1,val2,val3,val4]
>列表中的每一个元素都是可变的
>列表中的元素是有序的
>列表可以容纳 Python 中的任何对象

```python
all_in_list = [
    1,              #整数
    1.0,            #浮点数
    'a word',       #字符串
    print(1),       #函数
    True,           #布尔值
    [1,2],          #列表中套列表
    (1,2),          #元组
    {'key':'value'} #字典
]
```

#### 列表的增删改查

>insert() 插入

插入元素的实际位置在**指定位置元素之前,如果指定插入的位置在列表中不存在,那么这个元素一定会被放在列表的最后位置**

>remove() 删除

>extend() 添加多个元素

### 字典 dict = {key1,:val1,key2:val2}

>字典中数据必须是以键值对的形式出现的;
>逻辑上讲,键时不能重复的,而值可以重复;
>字典中的键(key)是不可变的,无法修改;值( value)是可变的,可修改,可以是任何对象

#### 字典的增删改查 (字典不能切片)

>update() 添加多个元素
>del 删除字典中的元素

### 元组 tuple = (val1,val2,val3,val4)

元组其实可以理解成为一个稳固版的列表,因为元组是不可修改的.

### 集合 set ={val1,val2,val3,val4}

每一个集合中的元素是无序的,不重复的任意对象
>add() 添加
>discard() 删除

### 数据结构的一些技巧

#### 多重循环

>sorted()
sorted 函数按照长短,大小,英文字母的顺序给每个列表中的元素进行排序.
**但是 sorted 函数并不会改变列表本身**
> zip()
如果同时需要整理两个列表: 

```python
for a,b in zip(num ,str)
    print(b,'is',a)
```

#### 推导式(列表解析式) list = [ item for item in iterable]

假设我现在有10个元素要装进列表中,普通写法:

```python
a = []
for i in range(1,11):
    a.append(i)
```

换成列表解析式写法:

```python
b = [ i for i in range(1,11)]
```

***列表解析式不仅方便,并且在执行效率上要远胜过前者***

#### 循环列表时获取元素索引 enumerate()

```python
letters = ['a','b','c','d','e','f','g']
for num ,letter in enumerate(letters):
    print(letter,'is',num+1)
```
## 类与可口可乐

```python
class CocaCola:
    formular = ['caffeine','sugar','water','soda']
```

### 类的实例化

```python
coke_for_me = CocaCola()
coke_for_you = CocaCola()
```

### 类属性引用 (类.属性名)

```python
print(CocaCola.formular)
>>> ['caffeine','sugar','water','soda']
for element in coke_for_me.formular:
    print(element)
>>> caffeine
>>> sugar
>>> water
>>> soda
```

#### 实例属性

创建了类后,通过 object.new_attr 的形式赋值,就得到了一个新的实例的变量.

```python
class CocaCola:
    formula = ['caffeine','sugar','water','soda']
    def drink(self):
        print('Energy!')
coke = CocaCola()
coke.drink()
>>> Energy!
```

一旦一个类被实例化,下面使用方式是相似的:

```python
coke = CocaCola
coke.drink == CocaCola.drink(coke) //两边一样
```

#### __init__()

```python
class CocaCola():
    formula = ['caffeine','sugar','water','soda']
    def __init__(self):
self.local_logo = '    ' def drink(self): # HERE 
        print('Energy!')
coke = CocaCola() print coke.local_logo)
```

### 类的继承

```python
class CaffeineFree(CocaCola):
    caffeine = 0
    ingredients =  [
        'High Fructose Corn Syrup',
        'Carbonated Water',
        'Phosphoric Acid',
        'Natural Flavors',
        'Caramel Color',
    ]
coke_a = CaffeineFree('Cocacola-FREE')
coke_a.drink()
```

我们在新类 CaffeineFree 后面的括号中放入 CocaCola ,这就表示这个类是继承与 CocaCola 这个父类; 类中的变量和方法可以完全被子类继承,可以对特殊改动进行覆盖.

### __dict__

__dict__是一个类的特殊属性,他是一个字典,用于储存类或者实例的属性.它是默认隐藏的.你不去定义它,他也会存在于每一个类中.
Python 中属性的引用机制是自外而内的,当你创建了一个实例后,引用属性的时候,编译器会先搜索该实例是否拥有该属性,如果有,则引用;如果没有,将搜索这个实例所属的类是否有这个属性,如果有,就引用,没有就只能报错了.

# Python 数据分析
