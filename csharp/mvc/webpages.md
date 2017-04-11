## WebPages

	webpages一般是有html和razor语法组成的页面

#### 导入内容@RenderPage
	
	<html>
	<body>
	@RenderPage("header.cshtml")
	<h1>Hello Web Pages</h1> 
	<p>This is a paragraph</p>
	@RenderPage("footer.cshtml")
	</body>
	</html>

#### _AppStart
* 隐藏敏感信息
	
	* 在 ASP.NET 中，隐藏敏感信息（数据库密码、电邮密码等）的常用方法是把这些信息保存在名为 "_AppStart" 的独立文件中。

	* _AppStart.cshtml
	
			@{
			WebMail.SmtpServer = "mailserver.example.com";
			WebMail.EnableSsl = true;
			WebMail.UserName = "username@example.com";
			WebMail.Password = "your-password";
			WebMail.From = "your-name-here@example.com";
			}

* Web 启动之前：_AppStart

	* 通过在站点根目录中创建名为 _AppStart 的页面，您能够在站点启动前就已执行启动代码（startup code）。假如该页面存在，ASP.NET 会在站点内的任何页面被请求之前首先运行这个页面。
	
	* 通常 _AppStart 用于启动代码和全局值的初始化，比如计数器和全局名称。
	* 注释 1：_AppStart 的文件扩展名应与网页相同，比如 _AppStart.cshtml。
	* 注释 2：_AppStart 有下划线前缀。正因如此，用户才无法直接查看该文件。

#### _PageStart
就像在站点启动前运行 _AppStart 那样，您能够在每个文件夹中的任意页面之前运行代码。
对于 web 中的每个文件夹，您能够添加名为 _PageStart 的文件。
通常 _PageStart 用于设置文件夹中所有页面的布局文件，或者在运行页面之前检查用户登录。

> ASP.NET 检查 _AppStart 是否存在。如果存在，并且这是对站点的第一次请求，则运行 _AppStart。
然后 ASP.NET 检查 _PageStart 是否存在。如果存在，则运行 _PageStart，在被请求的页面之前。
如果您在 _PageStart 内部引用了对 RunPage() 的调用，那么您可以规定在何处运行被请求的页面。否则，_PageStart 在被请求页面之前运行。
 
#### ~ 操作符
如需在编程代码中规定虚拟根目录，请使用 ~ 操作符。
如果您使用 ~ 操作符，而不是路径，就能够在不改变任何代码的情况下，将网站移动到不同的文件夹或位置：
	
	var myStyleSheet = "~/styles/StyleSheet.css";

#### Server.MapPath 方法
Server.MapPath 方法将虚拟路径 (/default.cshtml) 转换为服务器能够理解的物理路径 (C:\Johnny\MyWebSited\Demo\default.cshtml)。

	var pathName = "~/dataFile.txt";
	var fileName = Server.MapPath(pathName);

#### Href 方法
Href 方法把代码中的路径转换为浏览器能够理解的路径（浏览器无法理解 ~ 操作符）。

	@{var myStyleSheet = "~/Shared/Site.css";}
	
	<!-- 创建指向 CSS 文件的链接 -->
	<link rel="stylesheet" type="text/css" href="@Href(myStyleSheet)" />


#### Page 对象
* 方法
		
		href	使用指定的参数构建 URL。
		RenderBody()	在布局页中，将呈现不在指定部分中的内容页部分。
		RenderPage(page)	在其他页内呈现某一页的内容。
		RenderSection(section)	在布局页中，将呈现指定部分的内容。
		Write(object)	将指定的对象作为 HTML 编码的字符串写入。
		WriteLiteral	无需先对指定的对象进行 HTML 编码，即可将其写入。

* 属性

		isPost	返回一个值（true 或 false），指示客户端用来请求网页的 HTTP 数据传输方法是否为 POST 请求。
		Layout	获取或设置布局页的路径。
		Page	提供对页面、布局页和分页之间共享的页数据的类似属性的访问。
		Request	获得当前 HTTP 请求的 HttpRequest 对象。
		Server	获取 HttpServerUtility 对象，该对象所提供的方法能够在网页处理过程中使用。

* 其他常用的方法和属性

		Request.Cookies[key]	获得或设置 HTTP cookie 的值。
		Request.Files[key]	获取当前请求中被上传的文件。
		Request.Form[key]	获取表单中以 POST 方法发送的数据（以字符串形式）。
		Request.QueryString[key]	获取 HTTP 查询字符串变量集合。
		Request.Unvalidated(key)	有选择地禁用对表单元素、查询字符串值、cookie 的请求验证。
		Response.AddHeader()	将 HTTP 头添加到输出流。
		Response.OutputCache()	配置 HTTP 响应实例的缓存策略。
		Response.Redirect(path)	将客户端重定向到新的 URL。
		Response.SetStatus(StatusCode)	设置 HTTP 响应的 HTTP 状态代码。
		Response.WriteBinary()	将指定 MIME 类型的二进制内容的字节序列写入到 HTTP 响应输出流。
		Response.WriteFile(file)	将指定的文件直接写入 HTTP 响应输出流。
		@section(sectionName)	定义指定名称的内容部分。
		Server.HtmlDecode(htmlText)	对进行过 HTML 编码的字符串进行解码。
		Server.HtmlEncode(text)	对字符串进行 HTML 编码。
		Server.MapPath(virtualPath)	返回指定虚拟路径的服务器物理路径。
		Server.UrlDecode(urlText)	将已经为在 URL 中传输而编码的字符串转换为解码的字符串。
		Server.UrlEncode(text)	编码字符串，以便通过URL从服务器到客户端进行可靠的HTTP传输。
		Session[key]	获取或设置一个值，该值会一直存在直到用户关闭浏览器为止。
		ToString()	显示对象的值的字符串表示。
		UrlData[index]	获取与 URL 路径相关的数据。

#### Helper 辅助类
* WebGrid 快速构建html表格

		@{ 
		var db = Database.Open("SmallBakery") ; 
		var selectQueryString = "SELECT * FROM Product ORDER BY Id"; 
		var data = db.Query(selectQueryString); 
		var grid = new WebGrid(data); 
		}

		<div id="grid"> 
		@grid.GetHtml()
		</div> 

* Chart 帮助器能够显示数组、数据库或文件中的数据。
* WebMail 帮助器提供了使用 SMTP (Simple Mail Transfer Protocol) 来发送电邮消息的功能。
* WebImage 帮助器提供了在网页中管理图片的功能。
