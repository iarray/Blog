## Shell
### 变量
定义: `variableName="value"`

使用  `$variableName` 或 `${variableName}` 变量名外面的花括号是可选的

**推荐给所有变量加上花括号，这是个好的编程习惯。** 

重新定义变量不需要加`$`
	
	name='amy'
	echo ${name}
	name='mike'
	echo ${name}

变量设为只读 `readonly name`

删除变量 `unset name`

特殊变量
	
	$$  当前Shell进程ID。
	$0 	当前脚本的文件名
	$n 	传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1
	$#  传递给脚本或函数的参数个数。
	$* 	传递给脚本或函数的所有参数。
	$@ 	传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同
	$? 	上个命令的退出状态，或函数的返回值。

#### 替换
	
转义替换

	a=10
	echo -e "Value of a is $a \n"
	输出: Value of a is 10

如果不加 -e 则会原样输出 

	Value of a is 10\n 



变量替换
	
	${var} 变量本来的值
	${var:-word} 如果变量 var 为空或已被删除(unset)，那么返回 word，但不改变 var 的值。
	${var:=word} 如果变量 var 为空或已被删除(unset)，那么返回 word，并将 var 的值设置为 word。
	${var:?message} 如果变量 var 为空或已被删除(unset)，那么将消息 message 送到标准错误输出，可以用来检测变量 var 是否可以被正常赋值。
	若此替换出现在Shell脚本中，那么脚本将停止运行。
	${var:+word} 如果变量 var 被定义，那么返回 word，但不改变 var 的值。


### 运算符
#### 数学运算符

	a=10
	b=20

	val=`expr $a + $b` 
	val=`expr $a - $b` 
	val=`expr $a \* $b` 
	val=`expr $b / $a` 
	val=`expr $b % $a` 

注意:

1. 不要漏了`, 注意不是单引号,是esc下面的反引号
2. 乘号(*)前边必须加反斜杠(\)才能实现乘法运算

#### 关系运算符

	-eq 	检测两个数是否相等，相等返回 true。 (equal)
	-ne 	检测两个数是否相等，不相等返回 true。 (not equal)
	-gt 	检测左边的数是否大于右边的，如果是，则返回 true。 (greater than)
	-lt 	检测左边的数是否小于右边的，如果是，则返回 true。 (less than)
	-ge 	检测左边的数是否大等于右边的，如果是，则返回 true。 (greater or equal)
	-le 	检测左边的数是否小于等于右边的，如果是，则返回 true。 (less or equal)


#### 布尔运算符

	! 	非运算，表达式为 true 则返回 false，否则返回 true。 	 
	-o 	或运算，有一个表达式为 true 则返回 true。  
	-a 	与运算，两个表达式都为 true 才返回 true。 
举例

	if [ $a -lt 100 -o $b -gt 100 ]
	then
	   echo "$a -lt 100 -o $b -gt 100 : returns true"
	else
	   echo "$a -lt 100 -o $b -gt 100 : returns false"


#### 字符串运算符

	= 	检测两个字符串是否相等，相等返回 true。 	[ $a = $b ] 返回 false。
	!= 	检测两个字符串是否相等，不相等返回 true。 	[ $a != $b ] 返回 true。
	-z 	检测字符串长度是否为0，为0返回 true。 	[ -z $a ] 返回 false。
	-n 	检测字符串长度是否为0，不为0返回 true。 	[ -z $a ] 返回 true。
	str 检测字符串是否为空，不为空返回 true。 	[ $a ] 返回 true。

#### 文件测试运算符
	
	-b file 	检测文件是否是块设备文件，如果是，则返回 true。 
	-c file 	检测文件是否是字符设备文件，如果是，则返回 true。 
	-d file 	检测文件是否是目录，如果是，则返回 true。  
	-f file 	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 
	-g file 	检测文件是否设置了 SGID 位，如果是，则返回 true。 
	-k file 	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。 
	-p file 	检测文件是否是具名管道，如果是，则返回 true。 
	-u file 	检测文件是否设置了 SUID 位，如果是，则返回 true。 
	-r file 	检测文件是否可读，如果是，则返回 true。 
	-w file 	检测文件是否可写，如果是，则返回 true。 
	-x file 	检测文件是否可执行，如果是，则返回 true。 
	-s file 	检测文件是否为空（文件大小是否大于0），不为空返回 true。 
	-e file 	检测文件（包括目录）是否存在，如果是，则返回 true。 

举例:

	file="/var/www/tutorialspoint/unix/test.sh"
	if [ -r $file ]
	then
	   echo "File has read access"
	else
	   echo "File does not have read access"
	fi

### 注释
shell 只有单行注释, 注释以`#`开头

### 字符串
* 单引号里的任何字符都会原样输出,单引号字串中不能出现单引号(转义也不行)
* 双引号里可以有变量, 双引号里可以出现转义字符

取字符串长度 `${#string}`

提取子字符串 `${string:1:4}`, 从位置1开始截取4个字符(首字符索引是0)

查找子串
	
	echo `expr index "$string" is`


### 数组

    array_name=(value0 value1 value2 value3)

元素间用空格分开,使用下标赋值 `array_name[0]=value0`

取数组元素 `${array_name[2]}`

取数组长度 `${#array_name[@]}` 或 `${#array_name[*]}`


### 条件语句
#### if 语句

	if [ expression 1 ]
	then
	   Statement(s) to be executed if expression 1 is true
	elif [ expression 2 ]
	then
	   Statement(s) to be executed if expression 2 is true
	elif [ expression 3 ]
	then
	   Statement(s) to be executed if expression 3 is true
	else
	   Statement(s) to be executed if no expression is true
	fi

 最后必须以 fi 来结尾闭合 if，fi 就是 if 倒过来拼写

#### case语句

    echo 'Input a number between 1 to 4'
    echo 'Your number is:\c'
    read aNum
    case $aNum in
        1)  echo 'You select 1'
        ;;
        2)  echo 'You select 2'
        ;;
        3)  echo 'You select 3'
        ;;
        4)  echo 'You select 4'
        ;;
        *)  echo 'You do not select a number between 1 to 4'
        ;;
    esac

取值后面必须为关键字 `in`, 每一模式必须以**右括号**结束, 所有命令开始执行直至 `;;`。 `;; `与其他语言中的 break 类似, `esac`是case的倒写,表示结束case语句


### 循环语句

#### for语句

    for loop in 1 2 3 4 5
    do
        echo "The value is: $loop"
    done

#### while 语句

	while 表达式
	do
	   Statement(s) to be executed if command is true
	done

#### until 语句

	until [ ! $a -lt 10 ]
	do
	   echo $a
	   a=`expr $a + 1`
	done

until直至条件为 true 时停止。

#### break命令,允许跳出所有循环
####  continue命令,仅仅跳出当前循环。

### 函数 

	Hello () {
	   echo "say hi $1"
	   echo "say hi ${10}"
	   return "hello"; 
	}

不需要定义参数列表, 可以在函数内部使用$n (n是数字), 取得参数,例如 `$1` 取第一个参数,当n>=10时，需要使用`${n}`来获取参数。

函数调用:

	Hello "amy" "mike"

函数调用不需要括号, 参数间用空格分开