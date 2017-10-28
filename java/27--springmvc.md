## springmvc ##
	* 原理
		* 表现层框架，spring框架的一部分

	* 简单处理流程
		* 用户请求   ----> 前端控制器			---（请求业务处理）--->   处理器
		*              
		* 响应用户  <---（DispatherServlet） <---（返回处理结果）--- (Handler/Controller)
		*                  ↑         ↓          
		*              返回html   处理结果转发给jsp
		*                  ↑		 ↓
		*                    jsp页面

	* 架构
		* DispatcherServlet	前端控制器
		* HandlerMapping	处理器映射器
		* Handler			处理器（后端控制器）
		* HandlerAdapter	处理器适配器
		* ViewResolver		视图解析器
		* View				视图
		* 其中处理器映射器，处理器适配器，视图解析器被称为springmvc的三大组件

	* 流程
		* 用户发送请求到前端控制器DispatcherServlet
		* DispatcherServlet收到请求调用HandlerMapping处理器映射器
		* 处理器映射器找到具体的处理器，生成处理器对象及处理器拦截器
		* DispatcherServlet调用处理器适配器HandleAdapter
		* HandlerAdapter经过适配器调用具体的处理器（Controller,后端控制器）
		* Controller执行完成返回ModelAndView
		* DispatcherServlet将ModelAndView传给视图解析器ViewReslover
		* 视图解析器解析后返回具体的View
		* DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）
		* DispatcherServlet响应用户

	* web.xml中配置前端控制器
		* <!-- 前端控制器 -->
		*  <servlet>
		*     <servlet-name>springmvc</servlet-name>
		* 	  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		*    <init-param>
		*  		<param-name>contextConfigLocation</param-name>
		* 		<param-value>classpath:springmvc.xml</param-value>
		*  	 </init-param>
		*  </servlet>
		*  <servlet-mapping>
		* 	    <servlet-name>springmvc</servlet-name>
		*   	<url-pattern>/</url-pattern>
		*  </servlet-mapping>
	
	* springmvc.xml
		* 使用<mvc:annotation-driven/>替代注解处理器和适配器的配置
		
		* 注解式处理器适配器：RequestMappingHandlerAdapter
			* 对标记@RequestMapping(url)的方法进行适配
		
		* 组件扫描器：
			* <!-- 扫描controller注解,多个包中间使用半角逗号分隔 -->
			* <context:component-scan base-package=""/>
		
		* 视图解析器
			* <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			*		<!-- viewClass为默认值，可省略 -->
			*		<property name="viewClass"
			*			value="org.springframework.web.servlet.view.JstlView" />
			*		<property name="prefix" value="/WEB-INF/jsp/" />
			*		<property name="suffix" value=".jsp" />
			* </bean>



	* springmvc与struts2的区别
		* springmvc的核心是一个servlet,而struts2的核心是一个过滤器Filter
		* springmvc时基于方法开发（一个Url对应一个方法），请求参数传递到方法的形参，可以设计为单例或多例（建议单例）；Struts2是基于类开发，传递参数是通过类的属性，只能设计为多例
		* Struts2采用值栈存储请求和响应数据，通过OGNL存储数据；springmvc通过参数解析器是将request请求解析，并给方法形参复制，将数据和视图封装成ModelAndView对象，最后将该对象中的模型数据通过request域传给页面，jsp视图解析器默认使用jstl
 