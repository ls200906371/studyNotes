# 过滤器 #
	* 原理
		* 一个运行在服务器端的程序，先于与之相关的servlet或JSP页面之前运行，实现对其请求资源的过滤功能
		* 可附加到一个或多个servlet或JSP页面上，可以检查请求信息，也可以处理响应信息
		* 过滤器(Filter)的基本功能是对Servlet容器调用Servlet的过程进行拦截，从而在Servlet执行前后实现特殊功能

	* 编写过滤器的流程
		* 定义实现类实现javax.servlet.Filter接口，重写其方法
		* 编写配置文件web.xml，在其中加入：
			<filter>
				<filter-name></filter-name>
				<filter-class></filter-class>
			</filter>
			<filter-mapping>
				<filter-name></filter-name>
				<url-pattern>/*</url-pattern>(被拦截的资源地址，/*表示项目下的所有资源，包括其所有后代目录中的资源)
			</filter-mapping>

	* Filter生命周期
		* 服务器启动时，服务器就回创建过滤器对象，每次访问被拦截的资源时，过滤器中的doFilter方法1就回执行，当服务器关闭时，就回销毁过滤器对象
		* 服务器在启动时，执行过滤器的init(FilterConfig config)方法
		* 访问被拦截的资源时，执行doFilter(ServletRequest re,ServletResponse rs，FilterChain)方法
		* 服务器关闭时，执行destroy()方法

	* FilterConfig对象
		* 接口
		* getFlterName() : String
		* getServletContext() : ServletContext
		* getInitParameter(String name) : String
		* getInitParameterNames() : Enumeration<String>

	* FilterChain
		过滤器链
		* 过滤器的执行顺序跟<filter-mapping>的配置顺序有关
		* doFilter(ServletRequest re, ServletResponse rs)

	* 过滤器的配置之dispatcher
		<filter-mapping><dispather> ? </dispather></filter-mapping>
		* REQUEST	: 默认值，不拦截转发
		* FORWORD	: 拦截转发
		* ERROR		: 拦截跳转到错误页面，全局错误页面
		* INCLUDE	: 拦截在一个页面中跳转到另一个页面

# 监听器 Listener #
	对整个WEB环境的监听，当被监听的对象发生情况时，立即调用相应的方法进行处理
	* 编写监听器实现类实现指定的接口
	* 在web.xml中配置监听器（部分监听器不需要配置）
		<listener>
			<listener-class></listener-class>
		</listener>
	* 监听器接口
		* 域对象监听
			* ServletRequestListener
			* HttpSessionListener
			* ServletContextListener
		* 域对象属性监听
			* ServletRequestAttributeListener
			* HttpSessionAttributeListener
			* ServletContextAttributeListener

		* session作用域的特殊监听，无需配置web.xml文件
			* HttpSessionBindingListener	绑定/解绑
			* HttpSessionActivationListener	钝化/活化