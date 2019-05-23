输出控制符

常用的输出控制符主要有以下几个：

字符 | 对应数据类型 | 含义 | 示例  
-|-|-
d/i | int | 输出十进制有符号32bits整数，i是老式写法 | printf("%i",123);输出123
o | unsigned int | 无符号8进制(octal)整数(不输出前缀0) | printf("0%o",123);输出0173
u | unsigned int | 无符号10进制整数 | printf("%u",123);输出123
x/X | unsigned int | 无符号16进制整数，x对应的是abcdef，X对应的是ABCDEF（不输出前缀0x) | printf("0x%x 0x%X",123,123);输出0x7b 0x7B
f/lf | float(double) | 单精度浮点数用f,双精度浮点数用lf(printf可混用，但scanf不能混用) | printf("%.9f %.9lf",0.000000123,0.000000123);输出0.000000123 0.000000123。注意指定精度，否则printf默认精确到小数点后六位
F | float(double) | 与f格式相同，只不过 infinity 和 nan 输出为大写形式。 | 例如printf("%f %F %f %F\n",INFINITY,INFINITY,NAN,NAN);输出结果为inf INF nan NAN
e/E | float(double) | 科学计数法，使用指数(Exponent)表示浮点数，此处”e”的大小写代表在输出时“e”的大小写 | printf("%e %E",0.000000123,0.000000123);输出1.230000e-07 1.230000E-07
g | float(double) | 根据数值的长度，选择以最短的方式输出，%f或%e | printf("%g %g",0.000000123,0.123);输出1.23e-07 0.123
G | float(double) | 根据数值的长度，选择以最短的方式输出，%f或%E | printf("%G %G",0.000000123,0.123);输出1.23E-07 0.123
c | char | 字符型。可以把输入的数字按照ASCII码相应转换为对应的字符 | printf("%c\n",64)输出A
s | char* | 字符串。输出字符串中的字符直至字符串中的空字符（字符串以空字符’\0‘结尾） | printf("%s","测试test");输出：测试test
S | wchar_t* | 宽字符串。输出字符串中的字符直至字符串中的空字符（宽字符串以两个空字符’\0‘结尾） | setlocale(LC_ALL,"zh_CN.UTF-8");
wchar_t wtest[]=L"测试Test";
printf("%S\n",wtest);
输出：测试test
p | void* | 以16进制形式输出指针 | printf("%010p","lvlv");输出：0x004007e6
n | int* | 什么也不输出。%n对应的参数是一个指向signed int的指针，在此之前输出的字符数将存储到指针所指的位置 | int num=0;
printf("lvlv%n",&num);
printf("num:%d",num);
输出:lvlvnum:4
% | 字符% | 输出字符‘%’（百分号）本身 | printf("%%");输出:%
m | 无 | 打印errno值对应的出错内容 | printf("%m\n");
a/A | float(double) | 十六进制p计数法输出浮点数，a为小写，A为大写 | printf("%a %A",15.15,15.15);输出：0x1.e4ccccccccccdp+3 0X1.E4CCCCCCCCCCDP+3