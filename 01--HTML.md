# HTML #
定义：
	超文本标记语言，比文本功能强大，通过标签来包装数据
规范：
	HTML文件后缀为.html / .htm
	不区分大小写
	大部分标记通常包含开始标签和结束标签
	可以编写标签属性来修改数据显示效果
结构：
	<html>
		<head>描述信息，引入CSS和javaScript</head>
		<body>网页上显示的内容</body>
	</html>
排版标签：
	<p></p> / <hr/> / <br/>
字体标签：
	<font> </font>  color / size / face
	<h1></h1>...<h6></h6>
图片标签：
	<img /> src / width / height / alt
超链接标签：
	<a></a> href / target ="_blank / _parent / _self（默认）"
列表标签：
	有序
		<ol> type = "a / A / 1 / I" start
		 	<li></li> 
		</ol>
	无序
		<ul> type = "disc(默认) / circle / square"
			<li></li>
		</ul>
表格标签：
	<table></table> border / width / height / align / bgcolor / background ...
	<tr></tr>	...
	<td></td> rowspan / colspan ...
表单标签：
	<form></form>  action / method="get / post"
		<input /> name / value / type = "text / password / radio / checkbox / file
										 / hidden / submit / reset / button"	
	<select></select> name 下拉选择框
		<option></option>
	<textarea></textarea> 文本域输入框
框架标签：
	<frameset></frameset>   rows ="X%,*" / cols ="X%,*"  不能和<body></body>同时存在
		<frame /> src / name  切割后的页面