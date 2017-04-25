## Css

### A.选择器
1. 标签选择器
		
		p {}
		body {}

2. ID选择器

		<div id="content"> row 1 </div>
		#content { coloe:red; }

3. 类选择器(class)

		<div class="row"> row 1 </div>
		.row {}

4. 派生选择器

		li strong {
		    font-style: italic;
		    font-weight: normal;
		}

		<li><strong>我是斜体字。这是因为 strong 元素位于 li 元素内。</strong></li>

5. 属性选择器

		[title]
		{
			color:red;
		}
		<h2 title="Hello world">Hello world</h2>
		
		input[type="text"]
		{
		  width:150px;
		  display:block; 
		}
		<input type="text" name="Name" value="Bill" size="20">

		[title~=hello] { color:red; } //选择title 包含hello 的元素

6. 通用选择器(选择所有元素)

		* {}
		
* 选择器的分组

		h1,h2,h3,h4 { color: green; }

### B.继承
子元素从父元素继承属性,但在老式的浏览器,例如IE6,Netscape 4会忽略样式继承.

解决办法是手动设置一下,像下面这样

	body  {
	     font-family: Verdana, sans-serif;
	     }
	
	p, td, ul, ol, li, dl, dt, dd  {
	     font-family: Verdana, sans-serif;
	     }

如果不希望字体被所有的子元素继承,可以针对P写一个特殊样式

	p  {
     font-family: Times, "Times New Roman", serif;
     }

这样P就会摆脱了父元素的规则


### C.样式表 
1. 外部样式表 `<link rel="stylesheet" type="text/css" href="mystyle.css" />`
2. 内部样式表 `<style type="text/css"> hr {color: sienna;} </style>`
3. 内联样式 `<p style="color: sienna; margin-left: 20px">`

如果样式被多次定义,属性没有重复则会合并在一起,如果属性重复定义了,则按优先级覆盖

优先级: 内联样式 > 内部样式表 > 外部样式表


### D.样式
* [背景样式](http://www.w3school.com.cn/css/css_background.asp)
	* 背景图像 background-image: url(/i/eg_bg_03.gif); 
	* 背景定位 background-position:center;
	* 背景重复 background-repeat:no-repeat;
	* 背景固定 background-attachment:fixed;
	* 背景颜色 background-color:red;
* [文本样式](http://www.w3school.com.cn/css/css_text.asp)
	* 文本颜色 color:red;
	* 文本行高 line-height:5px; 
	* 缩进文本 text-indent: 5em;
	* 文本对齐 text-align:center; (text-align 不会控制元素的对齐，而只影响内部内容。)
	* 字间隔 word-spacing:30px;
	* 字母间隔 letter-spacing: 20px;
	* 大小写转换 text-transform: uppercase;(none/lowercase/uppercase/capitalize)
	* 文本装饰: text-decoration: none (underline下划线,overline删除线,line-through上划线)
	* 处理空白符 white-space: normal; (pre,nowrap,pre-wrap)
* [字体样式](http://www.w3school.com.cn/css/css_font.asp)
	* 字体 font-family: sans-serif;
		* 5 种通用字体
			* Serif 字体
			* Sans-serif 字体
			* Monospace 字体
			* Cursive 字体
			* Fantasy 字体
		* 其它字体 Georgia, 宋体 等
	* 字体风格 font-style:normal;	(italic文本斜体,oblique文本倾斜)
	* 文本加粗 font-weight:bold; (font-weight:900;)
	* 字体大小 font-size : 14px;
* [链接样式](http://www.w3school.com.cn/css/css_link.asp)

		a:link {color:#FF0000;}		/* 未被访问的链接 */
		a:visited {color:#00FF00;}	/* 已被访问的链接 */
		a:hover {color:#FF00FF; text-decoration:underline;}	/* 鼠标指针移动到链接上 */
		a:active {color:#0000FF;}	/* 正在被点击的链接 */ 
* [列表样式](http://www.w3school.com.cn/css/css_list.asp)
	* 列表项标志 list-style-type : square; (设置为方块)
	* 图片标志 list-style-image : url(xxx.gif);
* [边距](http://www.w3school.com.cn/css/css_margin_collapsing.asp)
	* 内边距 padding: 10px 5px 5px 10px  上、右、下、左内边距 
	* 外边距 margin: 5px 10px 5px 10px 上右下左
	* 外边距合并 

			#d1 {
			  width:100px;
			  height:100px;
			  margin-top:20px;
			  margin-bottom:20px;
			  background-color:red;
			}
			
			#d2 {
			  width:100px;
			  height:100px;
			  margin-top:10px;
			  background-color:blue;
			}
			
			</style>
			</head>
			
			<body>
			
			<div id="d1">
			</div>
			
			<div id="d2">
			</div>

			请注意，两个 div 之间的外边距是 20px，而不是 30px（20px + 10px）。
			即取得是 d1 的 margin-bottom
* 定位
	* 一切皆为框(div、h1 或 p 元素常常被称为块级元素。这意味着这些元素显示为一块内容，即“块框”。)
		* display 属性设置为 block，可以让行内元素（比如 `<a>` 元素）表现得像块级元素一样。
		* display 设置为 none 不占用文档中的空间。 
	* 定位机制,三种基本的定位机制：普通流(默认)、浮动和绝对定位。 
	* 定位类型 position : 相对定位,绝对定位,固定定位
		* [relative 相对定位](http://www.w3school.com.cn/css/css_positioning_relative.asp)

				h2.pos_left
				{
				position:relative;
				left:-20px
				}
				//相对正常元素左移20像素
		* [absolute 绝对定位](http://www.w3school.com.cn/css/css_positioning_absolute.asp)

				position:absolute;
				left:100px;
				top:150px
				}
				//距离页面左侧 100px，距离页面顶部 150px
		* fixed 固定定位

				{
				position:fixed;
				top:30px;
				right:5px;
				}
				//固定在距离窗口左侧5px,顶部30px
	* [浮动 float](http://www.w3school.com.cn/css/css_positioning_floating.asp) 
		* right  向右浮动时，它脱离文档流并且向右移动，直到它的右边缘碰到包含框的右边缘
		* left  向左浮动时，它脱离文档流并且向左移动，直到它的左边缘碰到包含框的左边缘。
		* 如果浮动元素的高度不同，那么当它们向下移动时可能被其它浮动元素“卡住” 
		* 想阻止行框围绕浮动框，需要对该框应用 clear 属性。clear 属性的值可以是 left、right、both 或 none，它表示框的哪些边不应该挨着浮动框。