## gcc

### 编译选项
#### 常用选项
gcc编译选项 | 选项的意义
-|-
-c | 编译、汇编指定的源文件，但是不进行链接
-S | 将C语言源文件编译为汇编语言，但是并不汇编该程序
-E | 预处理指定的源文件，不进行编译
-o [file1] [file2] | 将文件 file2 编译成可执行文件 file1
-I directory | 指定 include 包含文件的搜索目录
-g | 生成调试信息，该程序可以被调试器调试

#### 选项说明
* -c : 只编译不链接，仅生成目标文件(目标文件是一种中间文件或者临时文件，如果不设置该选项，gcc 一般不会保留目标文件，可执行文件生成完成后就自动删除了。)
```shell
$gcc -c test1.c test2.c test3.c
$ls -l *.o
-rwxr--r--  1  root  23  Feb  7  02:57  test1.o
-rwxr--r--  1  root  17  Feb  7  02:57  test2.o
-rwxr--r--  1  root  20  Feb  7  02:57  test3.o

```
* -o name : 告诉编译器可执行程序的名字
1. 将源文件作为输入文件,将可执行文件作为输出文件,编译完整的程序
```shell
gcc [infile] -o [outfile]
//例如
$ gcc main.c func.c -o app.out
```
2. 将源文件作为输入文件, 将目标文件作为输出文件, 即只编译不链接
```shell
$ gcc -c main.c -o a.o
```
3. 将目标文件作为输入文件，将可执行文件作为输出文件
```shell
gcc func.o main.o -o app.out
```
* -E 生成预处理文件
在编译之前，会先由预处理器进行一些文本替换方面的操作，例如宏展开、文件包含、删除部分代码等。
在正常的情况下，GCC 不会保留预处理阶段的输出文件，也即.i文件。然而，可以利用-E选项保留预处理器的输出文件，以用于诊断代码。-E选项指示 GCC 在预处理完毕之后即可停止。
```shell
//直接打印预处理信息
$ gcc -E main.c

//预处理信息输出到文件
$ gcc -E main.c -o main.i
```
* -I [path] : 引用非标准位置的头文件需要,需要在编译的时候指定这些头文件的位置
GCC 的-l选项可以让我们手动添加链接库。例如引用<math.h> 头文件。
```shell
$ gcc main.c -o main.out /usr/lib/libm.a
```

* -S 生成汇编语言
通常情况下，GCC 把汇编语言输出存储到临时文件中，并且在汇编器执行完后立刻删除它们。但是可以使用-S选项，让编译程序在生成汇编语言输出之后立刻停止。
```shell
$ gcc -S main.c
//生成汇编语言文件
main.s
```
如果想把C语言变量的名称作为汇编语言语句中的注释，可以加上-fverbose-asm选项：
```shell
$ gcc -S -fverbose-asm circle.c
```
