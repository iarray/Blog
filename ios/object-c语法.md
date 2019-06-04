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
-(void)init:(int)age;

// 类方法到声明(即静态方法)
// 语法:+(returnType)methodName:(typeName) var1 :(typeName)var2;
+(int)add:(int)a:(int)b;

@end

```
接口实现(以@implementation开头, @end结尾)
```object-c
@implementation MyClass
// 类方法定义
-(void)init:(int)age
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

