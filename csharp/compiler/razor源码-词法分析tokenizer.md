## 阅读Razor源码之CSharp词法分析器
	前景提要:最近对编译原理着迷了,各种找书找资料,也亲自实践做了一个词法
	分析器,无耐到语法分析那里又卡住了.书本都是以理论为主,代码描述并不多,
	想起ASP.NET里面有一个服务器端标记语言Razor,找了一下ASP.NET MVC的源
	码,还好这些年微软把很多优秀的框架开源了.借此机会学习一下微软的工程师是
	如何实现一个编译器前端的,废话不多说下面进入主题

### 1.源码获取
1. 首先安装git
2. 执行 `git clone https://git01.codeplex.com/aspnetwebstack.git`

下载下来文件有点多,这次我主要针对System.Web.Razor这个命名空间下的代码展开分析

### 2.目录结构
	System.Web.Razor
	|- Common 定义通用方法类
	|- Editor 编辑器的相关类(主要功能,代码自动完成,语法错误提示)
	|- Generator 代码生成(将.cshtml翻译为.cs文件)
	|- Parser 语法分析器
		|- SyntaxTree 语法树结构类
	|- Text 文本读写操作相关辅助类
	|- Tokenizer 词法分析器
		|- Symbols 符号及关键字的定义
	|- Utils 工具类

我们今天的主人公就是Tokenizer目录下的CSharpTokenizer.cs

### 3.源码分析
下面的内容我们假设读者已经掌握了有穷自动机的理论,如果没有这方面的理论知识,代码阅读起来可能会有点吃力.

* 什么是有穷自动机?
	
	有穷自动机是顾名思义是由有限个状态组成的,在有限个输入的情况下,在这些状态中转移并期望最终达到终止状态.详细解析可以阅读[自己动手开发编译器（三）有穷自动机](http://www.cnblogs.com/Ninputer/archive/2011/06/10/2077991.html)

* CSharpTokenizer.cs 中有穷状态机的代码描述

		private StateResult Data()
        {
			//IsNewLine方法是判断当前的字符是不是换行符
			//不同操作系统对换行符的定义不同,有的是\n,有的是\r\n,下面的代码也考虑到了
            if (ParserHelpers.IsNewLine(CurrentCharacter))
            {
                bool checkTwoCharNewline = CurrentCharacter == '\r';
				//TakeCurrent方法:吞了当前字符,并把位置标记移到下一字符
                TakeCurrent();
                if (checkTwoCharNewline && CurrentCharacter == '\n')
                {
                    TakeCurrent();
                }
				//EndSymbol结束本次符号的分析,符号的类型是换行符
				//返回终止状态
                return Stay(EndSymbol(CSharpSymbolType.NewLine));
            }
			//IsWhitespace方法是判断当前字符是否空格
            else if (ParserHelpers.IsWhitespace(CurrentCharacter))
            {
				//如果当前字符是空格,继续读取字符直到不是空格为止
                TakeUntil(c => !ParserHelpers.IsWhitespace(c));
                return Stay(EndSymbol(CSharpSymbolType.WhiteSpace));
            }
			//IsIdentifierStart判断当前字符符合标识符的首字符吗?(a-zA-Z_)
            else if (CSharpHelpers.IsIdentifierStart(CurrentCharacter))
            {	
				//进入标识符状态,解析标识符
                return Identifier();
            }
			//IsDigit判断当前字符是不是数字(0-9)
            else if (Char.IsDigit(CurrentCharacter))
            {
				//进入数字状态
                return NumericLiteral();
            }
			
			//如果起始状态下读入的第一个字符都不是上面的情况则,执行下面的代码
            switch (CurrentCharacter)
            {
                case '@':
                    return AtSymbol();
                case '\'':
					//解析字符
                    TakeCurrent();
                    return Transition(() => QuotedLiteral('\'', CSharpSymbolType.CharacterLiteral));
                case '"':
					//解析字符串
                    TakeCurrent();
                    return Transition(() => QuotedLiteral('"', CSharpSymbolType.StringLiteral));
                case '.': 
                    if (Char.IsDigit(Peek()))
                    {
						//如果.后面紧接的是数字,则尝试解析小数
                        return RealLiteral();
                    }
                    return Stay(Single(CSharpSymbolType.Dot));
                case '/':
                    TakeCurrent();
					//解析单行注释
                    if (CurrentCharacter == '/')
                    {
                        TakeCurrent();
                        return SingleLineComment();
                    }
					//多行注释
                    else if (CurrentCharacter == '*')
                    {
                        TakeCurrent();
                        return Transition(BlockComment);
                    }
					//  a/=1 
                    else if (CurrentCharacter == '=')
                    {
                        TakeCurrent();
                        return Stay(EndSymbol(CSharpSymbolType.DivideAssign));
                    }
                    else
                    {
                        return Stay(EndSymbol(CSharpSymbolType.Slash));
                    }
                default: 
					// 解析运算符(+,-,*,/,&,|等等)
                    return Stay(EndSymbol(Operator()));
            }
        }

为了方便阅读和理解,我直接在代码上面加上注释,而不是在代码下面加以说明,这是为了减少鼠标滚动的次数.上面的这段代码基本就把C#词法分析的大体轮廓勾画出来,状态机从起始状态读入一个符号后的状态可能有这几种:

1. 标识符(关键字也被识别为标识符)
2. 数字
3. 字符
4. 字符串
5. 操作符(运算符,注释符等等)
6. 空白字符(例如:空格)
7. 换行符
> 大多数程序语言支持在代码中穿插任意多的空格和换行符,一般的编译器在起始状态下遇到空白字符或换行符都会忽略


未完待续...

4/11/2017 5:03:31 PM 