## gcc

### 编译选项
* -o name : 告诉编译器可执行程序的名字
* -c : 只编译不链接，仅生成目标文件(目标文件是一种中间文件或者临时文件，如果不设置该选项，gcc 一般不会保留目标文件，可执行文件生成完成后就自动删除了。)
```shell
$gcc -c test1.c test2.c test3.c
$ls -l *.o
-rwxr--r--  1  root  23  Feb  7  02:57  test1.o
-rwxr--r--  1  root  17  Feb  7  02:57  test2.o
-rwxr--r--  1  root  20  Feb  7  02:57  test3.o

```

* -I path : 引用非标准位置的头文件需要,需要在编译的时候指定这些头文件的位置
* 