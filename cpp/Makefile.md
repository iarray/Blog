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
2. 