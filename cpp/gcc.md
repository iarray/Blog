## gcc

### 编译选项
#### 常用选项
gcc编译选项 | 选项的意义
-|-
-c | 编译、汇编指定的源文件，但是不进行链接
-S | 编译指定的源文件，但是不进行汇编
-E | 预处理指定的源文件，不进行编译
-o [file1] [file2] | 将文件 file2 编译成可执行文件 file1
-I directory | 指定 include 包含文件的搜索目录
-g | 生成调试信息，该程序可以被调试器调试

#### 选项说明
* -o name : 告诉编译器可执行程序的名字
1. 将源文件作为输入文件,将可执行文件作为输出文件,编译完整的程序
```shell
gcc [infile] -o [outfile]
//例如
$ gcc main.c func.c -o app.out
```

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