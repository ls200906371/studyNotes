# Servlet #
	* 页面定时跳转
		* 响应头 refresh
		* response.setHeader("refresh","5(5秒后跳转); url = /项目名/资源路径")
		
	* html文件定时跳转
		* 在<head>标签中添加标签 <meta http-equiv="refresh" content="5;url=/项目名/资源"/> 
	
	* 域对象
		由服务器创建的用来存储数据及进行数据传递的对象
		* 方法
			* setAttribute(String name,Object value) 存入
			* getAttribute(String name)		获取
			* removeAttribute(String name)	移除
			
		* ServletContext
			* 服务器启动时为每一个WEB应用都创建了一个ServletContext对象
			* 被应用中的所有的资源共享，单列对象
			* 服务器关闭时该对象才被销毁
			
		* 获取SevletContext对象
			* GenericServlet类中的getServletContext(),其子类直接继承
			
		* ServletContext获取全局的初始化参数
			* 全局的初始化参数
				* 在WEB.xml中添加
				  <context-param>
					<prama-name>XXX(变量名)</prama-name>
					<prama-value>yyy<变量值></param-value>
				  </context-param>	
				* getServletContext().getInitParameter("XXX")	
		
		* ServletContext读取文件
			* 方法
				* String getRealPath(String path)  获取绝对磁盘路径
				* InputStream getResourceAsStream(String name)	获取关联文件的输入流
			* String path 资源路径
				* 服务器端路径，无需写项目名
				* 看Tomcat服务器部署项目后的路径
					* 当文件在src目录下时，path = "/WEB-INF/classes/XXX"
	
	* HttpSerlvetRequest获取请求参数
		提交的表单中的输入项必须有name属性
		* 方法
			* String getParameter(String name)
			* String[] getParameterValues(String name)
			* Map getParameterMap()
			
		* 提交中文乱码
			* 原因
				* 页面为UTF-8编码，数据采用UTF-8编码，传递给Tomcat服务器
				* Tomcat服务器默认的编码是ISO-8859-1，通过该编码解码
			* 解决方法
				* 先使用ISO-8859-1进行编码，在再使用UTF-8编码
				* String value = new String(value.getBytes("ISO-8859-1"),"UTF-8")
			* post请求特有解决方法
				* 设置request对象的缓冲区编码
				* void setCharacterEncoding(String code)
		
		* 其它方法
			* String getContextPath()  --获取项目名
			* String getMethod()  --获取请求方式 GET / POST
			* String getHeader(String name)  --获取请求头信息(值)
	
	* BeanUtils工具类
		 协助服务器将客户提交给Servlet的数据进行封装
		* 导包
			* commons-logging-1.1.1.jar
			* commons-beanutils-1.8.3.jar
		
		* 开发步骤
			* 获取表单数据  Map<String,String[]> map = request.getParameterMap()
			* 创建要封装数据的javaBean类  User user = new User()
			* 使用BeanUtils工具类的方法封装数据  BeanUtils.populate(user,map)
		
	* 文件下载
		* 设置响应头 response.setHeader(String name.String value)
			* Content-Type	==	"XXX/YYY;charset=UTF-8"
				* String type = getServletContext().getMimeType(String file)	获取文件的mime类型value
			* Content-Disposition	==	"attachment;filename = 文件名"
			
		* 获取文件名
			超链接提交为get方法	
			* 设置超链接的name属性<a href = "/XXX?name=yyy"></a>
			* 获取到name属性为中文时进行解码编码	new String(yyy.getBytes("ISO-8859-1"),"UTF-8")
				
		* 获取文件的输入流
			* ServletContext
				* String getRealPath(String path) / new FileInputStream(String allPath)
				* InputStream getResourceAsStream(String path)
		
		* 获取输出流
			* ServletOutputStream  response.getOutputStream()  / PrintStream  response.getWriter()
			
	* 域对象HttpServletRequest
		* 生命周期：服务器接收请求时创建，服务器响应请求后销毁
		
	* 页面跳转之转发
		* 实现
			* ServletRequest的方法 : RequestDispatcher getRequestDispatcher(String path) path不包含项目名
			* RequestDispatcher的方法 : void forward(ServletRequest request,ServletResponse response)
			* request.getRequestDispatcher(path).forward(request,response) 简化
		
		* 转发和重定向区别
			* 地址栏不变，一次请求一次响应；重定向地址栏改变，两次请求，两次响应
			* 转发地址无需加项目名称，服务器路径；重定向需加项目名称，客户端路径
			* 转发可以使用request对象传递值；重定向不能
			* 转发只能在服务器内部操作，重定向可以定向任何资源



		
				 