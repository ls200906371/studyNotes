# JQuery #
	JavaScript库,封装了预定义对象和实用函数
	特点：
		* 轻量级，兼容多种浏览器,简化javascript的开发，使HTML页面的属性代码和内容分离

	引入JQuery步骤
		* 导入JQuery文件到工程中
		* 在JSP或者HTML页面引入JQuery的库,即.js文件
		* 在<head></head>中创建<script></script>标签中直接使用
	
	JQuery对象
		* 通过JQuery包装DOM对象后产生的对象，可以调用JQuery库的方法和属性，不能调用DOM对象的方法和属性
	
	JQuery对象和DOM对象之间的相互转换
		DOM --> JQuery
			* $(DOM对象) / JQuery(DOM对象)
		JQuery --> DOM
			* JQuery对象[0]
			* JQuery对象.get(0)
	JQuery的选择器
		* 基本选择器
			* 元素选择器  $("div")
			* 类选择器      $(".class")
			* ID选择器     $("#id")
		* 层级选择器
			* $(A B) 	A中的所有B，包含所有级子节点中的
			* $(A > B) 	A中一级子节点中的B
			* $(A + B) 	紧接着A之后的下一个同级节点B
			* $(A ~ B) 	A之后的所有同级节点B
	JQuery的过滤器
		* 基本过滤选择器   $("A:even")
			* :first	  	/ :last
			* :even	 偶数		/ :odd	奇数 从0开始
			* :eq(index) 等于/ :gt(index) 大于   / :lt(index) 小于下标值的元素从0开始
			* :not(A) 过滤A
			* :header 获取标题元素<h1></h1>...<h6></h6>
		* 属性过滤选择器   $("A[属性 = 值]")
			* [attribute]
			* [attribute = value]
			* [attribute != value]
			...
		* 可见过滤选择器 $("A:hidden")
			* :hidden   /  :visible
		* 表单过滤选择器   $(":input") 要有<form></form>
			* :input  包含textarea select input
			* :text
			* :file
			* ...
			* :hidden
			* :checked  复选框被选中的
			* :selected 下拉列表被选中的
	JQuery的常见方法
		* text()  / val()  / html() 
		* empty() / remove()
		* append() / appendTo()
		* each() / $.each()
		* attr() / css()
		* addClass() / removeClass()
		* show() slideDown() fadeIn() / hide() slideUp() fadeOut()
