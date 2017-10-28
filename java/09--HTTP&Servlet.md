# HTTP #
	超文本传输协议，（通信规则）
	* 请求协议	请求内容的格式
		* 请求行
			* 请求方式
				* get：会把请求参数显示到地址栏中，不安全，请求内容的大小有限制
				* post：把请求参数封装到请求正文中，比较安全，内容大小无限制
			* 请求路径
			* 协议版本 （1.0 、1.1（使用的））
		* 请求头
			* referer --记住当前的网页来源
			* user-agent --判断当前正在使用的浏览器
		* 空行
		* 请求体
	* 响应协议	响应内容的格式
		* 响应行
			* 协议版本
			* 状态吗 200 / 302 / 404 / 500
			* 状态码的描述
		* 响应头
			* refresh --页面的定时刷新
			* location --地址
			* content-disposition --在文件下载的时候，弹出下载的窗口
		* 空行
		* 响应体		封装响应的所有内容
		
# Servlet #
	一个小的java程序，运行在服务端，可以接收和响应从客户端发送过来的请求，使用HTTP协议
	* Servlet的开发
		* 编写java代码，继承Servlet的实现类
		* 编写配置文件，在web.xml文件中
			* <servlet>
					<servlet-name>XXX</servlet-name>
					<servlet-class>包名.类名</servlet-class>
			  </servlet>
			  <servlet-mapping>
			  		<servlet-name>XXX</servlet-name>
			  		<url-pattren>/yyy</url-pattern>
			  </servlet-mapping>
		
		* 配置自动加载的Servlet
			* 在<servlet></servlet>中添加标签
			  <load-on-startup>正整数(表示优先级，值越小，优先级越高)</load-on-startup>
		
		* Servlet的访问路径（优先级依次递减）
			* 完全路径匹配	/XXX
			* 目录匹配		/*
			* 扩展名匹配		*.xxx
	
	* Servlet的生命周期
		* 创建	第一次访问的时候该Servlet程序的时候
		* 提供服务	调用service()方法
		* 销毁	关闭服务器时
	
	* 路径
		* 客户端绝对路径
			* http://localhost:8080/项目名/资源
			* /项目名/资源 （简化）
		* 服务端绝对路径
			* /资源  （简化）
			
	* HttpServletRequest
		接口，代表请求，其实现类由服务器创建，父接口是ServletRequest
		* 获取请求参数的方法
			* String getParameter(String name)
			* String[] getParameterValues(String name) 用于获取复选框的值
			* Map getParameterMap() 获取表单中所有的值
		* 请求参数为中文时乱码的解决办法
			* get方法提交
				name = new String(name.getBytes("ISO-8859-1"),"UTF-8")
			* post方法提交
				request.setCharacterEncoding("UTF-8")
			
	* HttpServletResponse
		接口，代表响应，其实现类由服务器创建，父接口是ServletResponse
		* 向页面中输出内容
			* ServletOutputStream getOutputStream()
			* PrintWriter getWriter()
		* 输入中文乱码的解决办法
			* 字节流
				* response.setHeader("content-type","text/html;charset=UTF-8") 设置浏览器编码
				* out.write("中文".getBytes("UTF-8"))
			* 字符流
				* response.setHeader("content-type","text/html;charset=UTF-8")
				* response.setCharacterEncoding("UTF-8") 	设置response对象缓冲区编码
				* response.setContentType("text/html;charset=UTF-8") 简写
	
	* response重定向
		发送两次请求，得到两次响应
		* void setStatus(int sc) 设置状态码302
		* void setHeader("location","/项目名/资源") 设置响应头
		* void sendRedirect("/项目名/资源") 简化
	
	* response页面定时跳转
		* void setHeader(String name,String value)
		  response.setHeader("refresh","5;url=/项目名/资源")