## 切片

取集合前3个元素
	
	L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
	L[0:3]

从索引`0`开始取，直到索引`3`为止，但**不包括**索引`3`。即索引0，1，2，正好是3个元素。

也可以反向取
	
	>>> L[-2:]
	['Bob', 'Jack']
	>>> L[-2:-1]
	['Bob']

更多用法

	L = list(range(100))
前10个数，每两个取一个：
	
	>>> L[:10:2]
	[0, 2, 4, 6, 8]

所有数，每5个取一个：

	>>> L[::5]
	[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]

甚至什么都不写，只写[:]就可以原样复制一个list：

	>>> L[:]
	[0, 1, 2, 3, ..., 99]

tuple也是一种list，唯一区别是tuple不可变。因此，tuple也可以用切片操作，只是操作的结果仍是tuple

	>>> (0, 1, 2, 3, 4, 5)[:3]
	(0, 1, 2)

字符串也可以看成是一种list

	>>> 'ABCDEFG'[:3]
	'ABC'

Python没有针对字符串的截取函数，只需要切片一个操作就可以完成，非常简单

## 迭代
字典迭代

	d = {'a': 1, 'b': 2, 'c': 3}
	for key in d:
		print(key)

	输出: a b c
	
	for value in d.values():
		print(value)

	输出: 1 2 3 

	//同时取得key和value
	for k, v in d.items()

判断对象是否支持迭代
	
	from collections import Iterable
	isinstance('abc', Iterable) 

类似java的下标迭代

	for i, value in enumerate(['A', 'B', 'C']):
		print(i, value)

	结果:
	0 A
	1 B
	2 C

## 列表生成式
要生成`list [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`可以用`list(range(1, 11))`

如果要生成[1x1, 2x2, 3x3, ..., 10x10]怎么做？

	[x * x for x in range(1, 11)]
	结果:
	[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

要生成的元素`x * x`放到前面，后面跟for循环，就可以把list创建出来

for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方

	[x * x for x in range(1, 11) if x % 2 == 0]
	结果:
	[4, 16, 36, 64, 100]

还可以使用两层循环，可以生成全排列

	[m + n for m in 'ABC' for n in 'XYZ']
	结果:
	['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

当然还可以有三层,四层

列表生成式也可以使用两个变量来生成list

	d = {'x': 'A', 'y': 'B', 'z': 'C' }
	[k + '=' + v for k, v in d.items()]
	结果:
	['y=B', 'x=A', 'z=C']

把一个list中所有的字符串变成小写
	
	[s.lower() for s in ['Hello', 'World', 'IBM', 'Apple']]