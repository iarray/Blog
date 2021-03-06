Makefile由一组规则组成.每条规则的格式是:
```makefile
target ... : prerequisites ...
	command1
	command2
```

例如:
```makefile
main:func.o main.o
	gcc func.o main.o -o main

func.o:func.c func.h
	gcc -c func.c

main.o:main.c
	gcc -c main.c
```
main是这条规则的目标(Target),func.o main.o是这个规则的条件所有条件中只要有一个条件被更新了,目标也会随之被更新,所谓更新就是执行一边规则中的命令列表,命令列表每条必须以一个**Tab**开头, make会创建一个shell进程执行它.

需要执行更新的条件:
1. 目标没有生成
2. 某个条件修改时间比目标晚
3. 某个条件需要更新

---
通常makefile都会有一个clean规则,用于清理编译生成的二进制文件.
例如:
```makefile
clean:
	#注释
	@echo “cleaning ...”
	-rm main *.o
```
* 在命令前加 **-** 表示即使这条命令出错,make也会执行后续命令.因为有可能rm的文件不存在.一般mkdir和rm前面都加 **-** ,对于mkdir而言可能创建的目录已存在.
* 在命令前加@表示不输出命令本身, 只输出命令执行的结果
* #号开头表示注释

---
对于多目标规则,make会拆成多条单目标规则处理,例如:
```makefile
target1 target2:prerequisites1 prerequisites2
	command $< -o $@
```
相当于:
```makefile
target1:prerequisites1 prerequisites2
	command prerequisites1 -o target1
target2:prerequisites1 prerequisites2
	command prerequisites1 -o target2
```
这条规则命令列表是一样单,但是 $@ 不同

---
变量
先看下面一个例子
```makefile

a = $(b)
b = LiLi
all:
	@echo $(a)
```
执行make, 输出 
> LiLi

上面的例子可以看到a变量是定义在b变量前的,但是a变量的值还是被成功赋予了.
在执行规则all的时候才需要展开$(b),因此虽然b定义在a后面,但是展开的时候还是能取到$(b)的值

如果我们希望make在遇到变量的时候立即展开,可以在=号前加:(冒号)
上面的例子如果改成这样,则输出结果会是空白
```makefile
a := $(b)
b := LiLi
all:
	@echo $(a)
```
执行make, 输出空白内容

此外还有
* += 赋值可以在原有的变量值基础上追加内容
* ?= 如果变量没赋值则赋值,否则什么也不做
```makefile
a = Hello
a += LiLi
all:
	@echo $(a)
```
输出 Hello LiLi

---
特殊符号

* $@ 表示规则中的目标
* $< 表示规则中的第一个条件
* $? 表示规则中所有比目标新的条件,组成一个列表以空格分开
* $^ 表示规则中的所有条件,组成一个列表以空格分开
例如:
```makefile
main:main.o func.o
	gcc $^ -o $@
```

相当于:
```makefile
main:main.o func.o
	gcc main.o func.o -o main
```

---
用gcc -M或-MM 选项自动生成目标文件和源文件的依赖关系.
```makefile
gcc -MM main.c
```
输出
```language
main.o: main.c func.h
```

---
在make命令后面为变量赋值会覆盖makefile里面的
```shell
make CFLAGS=-g
```



