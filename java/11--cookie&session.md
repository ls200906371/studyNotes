# JSP #
	SUN公司提供了两种动态资源技术
	* Servlet和JSP
	* Java Server Pages java服务器端的网页，运行与服务器端
	* JSP中可以编写的内容
		* HTML / CSS /JavaScript
		* Java代码
		* JSP标签
	
	* JSP执行原理
		编写的JSP文件 -- 被服务器翻译成.java文件（输出HTML内容） -- 编译为.class文件 -- 执行

	* JSP的脚步元素
		在JSP的页面写java代码需要加脚步标签
		* 脚本标签
			* <%!  %>	--做全局变量出现，翻译为全局变量（一般不使用）
			* <%=  %>	--代表输出内容，不能用分号
			* <%  %>	--编写java语句(循环 判断等),定义局部变量

# Cookie #
	* 会话
		打开浏览器，点击多次超链接，访问多次资源，关闭浏览器，一次过程为一次会话
		* 解决的是每个用户产生的数据，把数据保存到会话对象中
		* 保存到其他对象不合适，由于生命周期，共享性等不合适

	* Cookie的概述
		* 是客户端的技术，最终被保存到浏览器上
		* 程序把每个用户的数据以cookie的形式写个用户各自的浏览器上
		* 当用户使用浏览器再去访问WEB资源时，也会带着Cookie去

	* Cookie原理
		* 基于客户端的技术，由服务器创建，默认保存在客户端浏览器上
		* 基于Http协议，set-cookie 响应头 / cookie 请求头
		* 可以用于客户端和服务端的数据传递
	
	* 方法
		* Cookie(String name,String value)   --没有无参构造，不支持中文
		* String getName()	--获取Cookie名
		* String getValue()	--获取Cookie中存入的值
		* void setValue(String value) --设置值

		* void setMaxAge(int expiry)
			设置Cookie的有效时间，如果浏览器关闭了，默认Cookie就被清除了
		* void setPath(String url)
			设置有效路径，不设置时默认为：/项目名
	* 操作Cookie对象的方法
		* request: Cookie[] getCookies()	获取浏览器返回的Cookie
		* response: void addCookie(Cookkie cookie)	向浏览器发送Cookie

# HttpSession #
	* 域对象，服务器端技术，使用时简称为session对象
	* 服务器在运行时为每一个用户的浏览器创建一个其独享的session对象，用户在访问web资源时，可以把各自的数据存入其中
	* 当用户再去访问其它web资源时，服务器可以从用户的session对象中取出数据为用户服务
	* session基于cookie传递。传递的是session的id值

	* 方法
		* void setAttribute(String name,String value)
		* Object getAttribute(String name)
		* void removeAttribute(String name)
		* String getId()	在服务器端session有唯一id值，获取该id值
		* void invalidate()		销毁session对象
		* ServletContext getServletContext()	获取ServletContext域对象

		* request.getSession()	--获取session对象，不存在时创建

	* Session对象的创建和销毁
		* Session的创建
			* request.getSession() --如果cookie对象中有jsessionid的cookie，就直接查找，否则创建

		* 销毁
			* 非正常关闭服务器，立即销毁
			* 正常关闭，session会被序列化到磁盘中
			* session的默认销毁时间为30分钟(没有操作时)
			* 调用方法invalidate（） 销毁

		* 配置session的销毁时间
			* 在tomcat/conf/web.xml文件中设置了session的默认超时时间
				<session-config>
					<session-tiomeout>30</session-timeout>
				</session-config>
			* 设置session的最大存活时间
				* void setMaxInactiveInterval(int intval)

