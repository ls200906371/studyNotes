# AJAX #
	Asynchronous Javascript and XML(异步Javascript和XML)，可以使网页实现异步更新
	* 原理
		* 使用Javascript获得浏览器内置的AJAX引擎(XMLHttpRequest对象)
		* 通过AJAX引擎确定请求路径和请求参数
		* 通过AJAX引擎发送请求
		* AJAX会在不刷新浏览器地址栏的情况下，发送请求
		* 服务器获得请求参数
		* 服务器处理请求参数
		* 服务器响应数据给请求参数
		* 通过设置给AJAX的回调函数获得服务器响应的数据
		* 使用JavaScript在指定的位置，显示响应数据，达到局部刷新的目的

	* JQuery调用AJAX
		* $.post(url, [data], [callback], [type])
			* url : 请求路径
			* data : 请求参数
			* callback : 回调函数
			* type : 返回内容格式，包括 xml / html / script / text / json / _default
				* 回调函数的参数为json类型时，服务器响应编码为：application/json;charset=UTF-8
				* 回调函数的参数为字符串类型时，服务器响应编码为：text/html;charset=UTF-8

		* $.get(url, [data], [callback], [type])
			* 除了请求方式不同，使用方式通$.post()

	* JSON数据
		JavaScript Object Notation 一种轻量级的数据交换格式，完全独立于语言的文本格式，即不同的编程语言JSON数据一致
		* JSON格式
			* JSON对象
				{"key":"value","key":"value", ...} key必须使用双引号，value不使用双引号表示变量
			* JSON数组
				{obj, obj, ...}

# JSON-LIB #
	将java对象与json数据相互交换的工具
	* 常用对象
		* JSONObject : java对象(JavaBean 、Map)与JSON数据转换
		* JSONArray : java集合、数组(List、Array)与JSON数据转换