## 阅读ASP.Net MVC 源码之 CSharp词法分析器
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
            if (ParserHelpers.IsNewLine(CurrentCharacter))
            {
                // CSharp Spec §2.3.1
                bool checkTwoCharNewline = CurrentCharacter == '\r';
                TakeCurrent();
                if (checkTwoCharNewline && CurrentCharacter == '\n')
                {
                    TakeCurrent();
                }
                return Stay(EndSymbol(CSharpSymbolType.NewLine));
            }
            else if (ParserHelpers.IsWhitespace(CurrentCharacter))
            {
                // CSharp Spec §2.3.3
                TakeUntil(c => !ParserHelpers.IsWhitespace(c));
                return Stay(EndSymbol(CSharpSymbolType.WhiteSpace));
            }
            else if (CSharpHelpers.IsIdentifierStart(CurrentCharacter))
            {
                return Identifier();
            }
            else if (Char.IsDigit(CurrentCharacter))
            {
                return NumericLiteral();
            }
            switch (CurrentCharacter)
            {
                case '@':
                    return AtSymbol();
                case '\'':
                    TakeCurrent();
                    return Transition(() => QuotedLiteral('\'', CSharpSymbolType.CharacterLiteral));
                case '"':
                    TakeCurrent();
                    return Transition(() => QuotedLiteral('"', CSharpSymbolType.StringLiteral));
                case '.':
                    if (Char.IsDigit(Peek()))
                    {
                        return RealLiteral();
                    }
                    return Stay(Single(CSharpSymbolType.Dot));
                case '/':
                    TakeCurrent();
                    if (CurrentCharacter == '/')
                    {
                        TakeCurrent();
                        return SingleLineComment();
                    }
                    else if (CurrentCharacter == '*')
                    {
                        TakeCurrent();
                        return Transition(BlockComment);
                    }
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
                    return Stay(EndSymbol(Operator()));
            }
        }

未完待续...

4/11/2017 5:03:31 PM 