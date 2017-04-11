### C# 的主要 Razor 语法规则
* Razor 代码封装于 @{ ... } 中
* 行内表达式（变量和函数）以 @ 开头
* 代码语句以分号结尾
* 字符串由引号包围
* C# 代码对大小写敏感
* C# 文件的扩展名是 .cshtml

### 示例代码

	<!-- 单行代码块 -->
	@{ var myMessage =	"Hello World"; }
	
	<!-- 行内表达式或变量 -->
	<p>The value of myMessage is: @myMessage</p> 
	
	<!-- 多行语句代码块 -->
	@{
	var greeting = "Welcome to our site!";
	var weekDay = DateTime.Now.DayOfWeek;
	var greetingMessage = greeting + " Here in Huston it is: " + weekDay;
	}
	<p>The greeting is: @greetingMessage</p>

### for循环

	<html>
	<body>
	@for(var i = 10; i < 21; i++)
	    {<p>Line @i</p>}
	</body>
	</html>


### if-else

	@{var price=20;}
	<html>
	<body>
	@if (price>30)
	  {
	  <p>The price is too high.</p>
	  }
	else
	  {
	  <p>The price is OK.</p>
	  } 
	</body>
	</html>


从编译原理角度来看,解析cshtml文件的主要实现方法是遇到razor语法则输出为c#代码,遇到html输出为Response.Write(html文本),最终生成一般处理程序(ashx)