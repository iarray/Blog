## Object-c语法
### Objective-C代码的文件扩展名
扩展名 | 内容类型
-|-
.h | 头文件。头文件包含类，类型，函数和常数的声明。
.m | 源代码文件。这是典型的源代码文件扩展名，可以包含 Objective-C 和 C 代码。
.mm | 源代码文件。带有这种扩展名的源代码文件，除了可以包含Objective-C和C代码以外还可以包含C++代码。仅在你的Objective-C代码中确实需要使用C++类或者特性的时候才用这种扩展名。

### 关键字
object-c的关键字均以@符号标注

### 接口和实现
在oc中一般类就是指接口,接口定义一般在.h文件中, 接口实现一般在.m文件中

接口声明(以@interface开头, @end结尾)
```object-c
@interface MyClass:NSObject{ 
  // 类变量声明
  @private //变量的属性
    int age;
}
// 类属性声明
@property(copy) int age;

// 实例方法
// 语法:-(returnType)methodName:(typeName) var1 :(typeName)var2;
-(void)call:(int)age;

// 类方法到声明(即静态方法)
// 语法:+(returnType)methodName:(typeName) var1 :(typeName)var2;
+(int)add:(int)a:(int)b;

@end

```
接口实现(以@implementation开头, @end结尾)
```object-c
@implementation MyClass
// 类方法定义
-(void)call:(int)age
{

}
@end
```

### 函数调用
在object-c中,函数调用使用中括号包住,语法如下
> [instanceName methodName:var1:var2]

例如:
```object-c
[ctx method:5];
```
相当于java, c++
```java
ctx.method(5);
```

### self & super
object-c中到self代表自身, 相当于java到this
super代表父类,和java一样

### 构造函数和析构函数
在C++中的类构造和析构函数和类名相关,而在object-c中,构造函数和析构函数是固定的
```object-c
-(id) init
{
    if (self = [super init])
    {

    }
    return self;
}

-(void) dealloc
{
    [super dealloc];
}
```

### SmallTalk的消息传递语法风格
Objective-C的面向对象语法源自SmallTalk，消息传递（Message Passing）风格。在源码风格方面，这是它与C Family语言（包括C/C++、Java、PHP）差别最大的地方。

在Java、C++世界，我们调用一个对象的某方法，在Objective-C里，这称作给类型发送一个消息，这可不仅仅是文字游戏，他们的技术细节也是不同的。

在Java里，对象和方法关系非常严格，一个方法必须属于一个类/对象，否则编译是要报错的。而在Objective-C里，类型和消息的关系比较松散，消息处理到运行时（runtime）才会动态决定，给类型发送一个它无法处理的消息，也只会抛出一个异常，而不会挂掉。
```language
[obj undefinedMethod];
```
在代码里调用没定义的方法（这是Java世界的习惯说法啊，专业的叫法是，给obj对象传递它无法处理的消息），Xcode会警告，但编译能成功，运行的时候会出错。它会输出这样一个错误：
```err
Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[NSObject undefinedMethod]: unrecognized selector sent to instance 0x8871710'
```
### 字符串
Objective-C里有字符串是由双引号包裹，并在引号前加一个@符号，例如：
```language
title = @"Hello";
if(title == @"hello") {}
```
