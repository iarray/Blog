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

