## Object-c语法
### Objective-C代码的文件扩展名
扩展名 | 内容类型
-|-
.h | 头文件。头文件包含类，类型，函数和常数的声明。
.m | 源代码文件。这是典型的源代码文件扩展名，可以包含 Objective-C 和 C 代码。
.mm | 源代码文件。带有这种扩展名的源代码文件，除了可以包含Objective-C和C代码以外还可以包含C++代码。仅在你的Objective-C代码中确实需要使用C++类或者特性的时候才用这种扩展名。

### 关键字
object-c的关键字均以@符号标注

### 字符串
Objective-C里有字符串是由双引号包裹，并在引号前加一个@符号，例如：
```language
title = @"Hello";
if(title == @"hello") {}
```


## 类
### 接口和实现
在oc中一般类就是指接口,接口定义一般在.h文件中, 接口实现一般在.m文件中（也有.mm的扩展名，表示Objective-C和C++混编的代码）。

接口声明(以@interface开头, @end结尾)
写在.h头文件里的方法都是公开的，Objective-C里没有私有方法的概念

**MyClass.h**
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

**MyClass.m**
```object-c
@implementation MyClass
// 类方法定义
-(void)call:(int)age
{

}
@end
```

### 继承
继承是写在Interface定义里面的。语法为：子类名在左，父类名在右，中间用冒号分隔。 
```language
@interface MyClass : NSObject
@end
```


### 构造实例
```language
mycls = [[MyClass alloc] init];
//或者
mycls = [MyClass new];

[mycls sayHello];
```
* alloc：分配内存。
* init：初始化。
* new：代替上面两个函数：分配内存，并且初始化。

### 函数调用
前文述及，不涉及面向对象时，它和C是完全一样的。以下是几个函数调用的示例：
不带参数
```language
startedBlock();
```

#### 传递消息给类/实例方法
函数调用使用中括号包住,语法如下
```shell
//不带参数
[instanceName methodName]

//带两个参数
[instanceName methodName:var1:var2]
```

例如:
```object-c
[ctx method:5];
```
相当于java, c++
```java
ctx.method(5);
```
#### 带多个参数
```language
- (void) setColorToRed: (float)red Green: (float)green Blue:(float)blue {...} //定义方法
[myObj setColorToRed: 1.0 Green: 0.8 Blue: 0.2]; //调用方法
```

#### 消息嵌套
```language
UINavigationBar *bar = [[[UINavigationBar alloc] init] autorelease];
```
对应java版本
```java
UINavigationBar bar = UINavigationBar.alloc().init().autorelease();
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

### Selector
selector就是一个方法指针
#### 绑定控件触发的动作
```language
@implementation DemoViewController
- (void)downButtonPressed:(id)sender {//响应“按钮被按下事件”的方法
	UIButton *button = (UIButton*)sender;
	[button setSelected:YES];
}

- (void)drawAnButton {
	UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom]; 
	btn.frame = _frame; 
	btn.tag = 1;
	btn.backgroundColor = [UIColor clearColor];
	[btn addTarget: self
		 action: @selector(downButtonPressed:)
		 forControlEvents: UIControlEventTouchUpInside];//当这个按钮被按下时，触发downButtonPressed:方法
}
@end
```
