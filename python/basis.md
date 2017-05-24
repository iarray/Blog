## 变量
python 中定义变量不需要任何关键字,变量可以改变数据类型

	a=1
	a='123'

空值 `None`
 
两种除法

* `/` 除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数
* `//` 除法只取结果的整数部分

	
> Python的整数没有大小限制,Python的浮点数也没有大小限制，但是超出一定范围就直接表示为inf（无限大）


### 字符串与编码
Python提供了`ord()`函数获取字符的整数表示，`chr()`函数把编码转换为对应的字符
	
	>>> ord('A')
	65
	>>> ord('中')
	20013
	>>> chr(66)
	'B'
	>>> chr(25991)
	'文'

`len()`函数计算的是str的字符数，如果换成bytes，`len()`函数就计算字节数：
	
	>>> len('中文')
	2
	>>> len('中文'.encode('utf-8'))
	6

由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-
并且要确保文本编辑器正在使用`UTF-8 without BOM`编码

#### UTF-8与Unicode
`ASCII`编码长度是一个字节所以只能表达2^8种字符即256个,而`Unicode`编码是双字节,能表达2^16种字符,所以Unicode能描述世界各地的所有文字.

由于`Unicode`是固定双字节长度,在表达`ASCII`范围内的字符时,真正用到的只有一个字节,高8位被填充为0,在存储以英文为主的文本时就会造成空间的浪费,所以`UTF8`诞生了.

`UTF8`是可变长编码,常用的英文字母被编码成1个字节，汉字通常是3个字节


----------


## 列表
list和tuple是Python内置的有序集合，一个可变，一个不可变。根据需要来选择使用它们。
### list

	classmates = ['Michael', 'Bob', 'Tracy']

用`len()`函数可以获得list元素的个数

用`-1`做索引，取最后一个元素, `-2` 可以取倒数第二个,以此类推

`append`在末尾追加元素, `insert` 将元素插入到指定位置

	classmates.append('Adam')
	classmates.insert(1, 'Jack')

`pop`可以删除列表元素

	classmates.pop(); 	//删除末尾元素
	classmates.pop(0)	//删除第一个元素

list里面的元素的数据类型可以不同

    L = ['Apple', 123, True]
    s = ['python', 'java', ['asp', 'php'], 'scheme']

### tuple
tuple是只读列表,一旦初始化就不能修改

	classmates = ('Michael', 'Bob', 'Tracy')

tuple里面存放引用对象的话,引用对象的内容是可以修改的

	t = ('a', 'b', ['A', 'B'])
	t[2][0] = 'X'
	t[2][1] = 'Y'
	结果: ('a', 'b', ['X', 'Y'])

## 循环
两种方式 `for x in list` 或者 `while 条件表达式`

	sum = 0
	for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
	    sum = sum + x

	for x in range(101):
    	sum = sum + x

	n = 99
	while n > 0:
	    sum = sum + n
	    n = n - 2


> python靠缩进来划分代码块

## 字典
dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

### dict
字典定义与使用

	d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
	d['Adam'] = 67	//往字典添加数据(或修改现有数据)
	
	'Thomas' in d	//判断key是否存在
	d.pop('Bob')	//删除key

和list比较，dict有以下几个特点：

    查找和插入的速度极快，不会随着key的增加而变慢；
    需要占用大量的内存，内存浪费多。

而list相反：

    查找和插入的时间随着元素的增加而增加；
    占用空间小，浪费内存很少。

所以，dict是用空间来换取时间的一种方法。

### set 

	s = set([1, 2, 3])
	s.add(4)	//添加元素到set中，可以重复添加，但不会有效果
	s.remove(4)	//删除元素
	

## 函数 
定义

	def abs(x):
	    if x >= 0:
	        return x
	    else:
	        return -x

多值返回

	def move(x, y, step, angle=0):		//angle默认值0
	    nx = x + step * math.cos(angle)
	    ny = y - step * math.sin(angle)
	    return nx, ny

	x, y = move(100, 100, 60, math.pi / 6)

常用函数-数据类型转换函数

	>>> int('123')
	123
	>>> int(12.34)
	12
	>>> float('12.34')
	12.34
	>>> str(1.23)
	'1.23'
	>>> str(100)
	'100'
	>>> bool(1)
	True
	>>> bool('')
	False
