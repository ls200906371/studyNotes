## HTML定义网页骨架，CSS美化显示效果，JS定义动态显示效果，实现用户和网页的交互 ##
# HTML #
超文本标记语言
	<div></div> 声明一块区域，尾部换行,默认左上角对其，占一行
	<span></span> 声明一块区域，不自动换行，
# CSS #
定义
	层叠样式表
	作用于HTML的标签上，修饰HTML的显示，将HTML的标签和属性分离，即页面元素和样式分离，代码复用。
CSS的引入方式：
	行内样式：直接在html的标签中的style属性内编写：
		<div style="color:red;font-size:10px;">123</div>
	内部样式：在html的<head>标签中使用<style>标签来定义CSS:
		<style>
			div{
				color:red;
				font-size:20px;
				...
			}
		</style>
	外部样式：将CSS定义成一个.css文件，在html中将该文件引入到html中，写在<head>标签内：
		<link href="style.css" rel="stylesheet" type="text/css"/>
CSS的基本选择器
	方便更精确的找到某个元素来设计，以下优先级依次递增
	元素选择器，即标签选择器
	类选择器,属性class .class{}
	id选择器，属性id #id{}
CSS的扩展选择器
	关联选择器 <div> <font> {} 选择标签<div>中的<font>
	组合选择器 <div>,<span> {} 选择多个标签，中间用逗号隔开
CSS的悬浮
	float属性：left / right
	clear属性：清除浮动 left / right / both
CSS的定位
	position
	style="position:absolute;top:100px;left:100px;"
CSS的盒子模型
	外边距：盒子边框与盒子所在区域的边框之间的距离
		margin / Margin-top / Margin-right / Margin-bottom / Margin-left
	内边距：盒子内的元素与盒子边框之间的距离
		padding /Padding-top / Padding-right / Padding-bottom / Padding-left

# JavaScript #
定义
	基于对象和事件驱动的脚本语言，主要作用于客户端，即浏览器上，弱类型语言。
结构
	ECMAScript:统一的标准语法、语句
	DOM:浏览器对象模型
	BOM:文档对象模型
特点
	交互性：客户与浏览器的交流
	安全性：不能访问本地磁盘
	扩平台性：只要有浏览器就能运行，不管在什么平台
结合方式
	内部方式：在标签<script></script>内写入JS代码
	外部方式：引入外部JS文件，<script src="JS文件路径">中间不再定义任何代码</script>
语法
	1 单词区分大小写
	2 定义变量，使用关键字var，var 变量名 = 变量（可以是任意类型）
	3 基本数据类型：
		string  / number / boolean / undefined / null
	4 引用数据类型：
		基于对象而不是面向对象，内置对象，对象的默认类型是null，
	5 定义方法用到关键字function：function xxx() {}
	6 获取指定标签：document.getElementById("标签的id属性值")
	  获取标签中的属性：document.getElementById("标签的id属性值").src="???" / .value;
	7 设置定时器：window对象中的方法
		setInterval("定时执行的方法名()",执行周期时间毫秒值) 重复执行
		setTimeout("定时执行的方法名()",执行的时间毫秒值) 在指定的时间后调用一次
	  清除定时器：
		clearInterval()
		clearTimeout()
JS事件
	先有事件源，绑定事件，用到的属性
		onclick  点击事件 <input type="button" value="按钮" onclick="执行的方法名()">
		onsubmit 提交事件 <form onsubmit="return 执行的方法名()"></form>
		onload   加载事件 <body onload="执行的方法名()"></body>