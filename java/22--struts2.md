# Struts2 #
	* 原理
		* Struts2是一个基于MVC设计模式的Web应用框架，本质上相当于一个Servlet,在MVC设计模式中，Struts2作为控制器来建立模型与视图的数据交互。Struts2以WebWork为核心，采用拦截器的机制来处理用户的请求，使业务逻辑控制器能够与ServletAPI完全脱离开

	* MVC设计模式
		* Model 模型，封装数据	javabean
		* View 视图，展示界面		jsp
		* Controller 控制器，控制程序流程	Servlet

	* 搭建Struts2的开发环境
		* 在WEB工程中引入Struts2的开发包
		
		* 编写Struts的配置文件
			* 在类的根路径下创建一个名为struts.xml的文件
			* 导入dtd约束，该约束在struts-core-xxxx.jar中
				* <!DOCTYPE struts PUBLIC
					"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
					"http://struts.apache.org/dtds/struts-2.3.dtd">

		* 配置Struts的前端控制器（核心过滤器）
			* 在web.xml文件中配置过滤器
			* <filter>
       			 <filter-name>struts2</filter-name>
        		 <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
   			  </filter>
   			  <filter-mapping>
        		<filter-name>struts2</filter-name>
       		    <url-pattern>/*</url-pattern>
   			  </filter-mapping>

		* 编写结果视图（jsp页面）
			* 访问动作类中的动作方法之后要跳转的页面
			* <result name="success">success.jsp</result>
			
		* 编写动作类和动作方法
			* 动作类
				* 一个简单的java类
				
				* 无侵入式的定义
					* 定义一个普通的java类，提供一些符合要求的动作方法

				* 侵入式的定义
					* 实现一个Action接口
						* 接口中的常量
							* SUCCESS / NONE / INPUT / LOGIN / ERROR
						* 接口中的方法
							* execute() : 默认的动作方法，不指定动作方法时由它执行

					* 继承一个ActionSupport类，它实现了Action接口（推荐）
						* 默认的动作类，在struts-default.xml中定义，全类名为com.opensymphony.xwork2.ActionSupport
						* 在action元素中不指定动作类时，默认为它，执行的动作方法为execute()

			* 动作方法
				* 必须是public修饰的
				* 有返回值且必须是String类型的，返回值必须跟struts.xml中的result元素的name属性的值一致
				* 不能有参数列表
				
	* Struts2的执行流程
		* 明确开发struts2应用要做的三件事
			* 写配置文件
			* 写动作类和动作方法
			* 写结果视图

	* Struts的配置文件
		* struts2提供了两种配置的方式，一种是.properties，一种是.xml，推荐使用xml文件，因为它可以描述层级关系

		* 加载时机
			* 当应用被tomcat加载的时候，struts2的配置文件已经被加载过了
			
		* 加载顺序(由上到下依次加载)
			* 三个默认配置文件，不能修改
				* default.properties
				* struts-default.xml
				* struts-plugin.xml

			* 应用中的配置文件
				* struts.xml
				* struts.properties
				* web.xml（文件中属于web部分的服务器启动就加载，属于struts2部分的到此时才加载）
				
		* Struts2中的常量
			* struts.i18n.encoding		UTF-8		应用中使用的编码
			* struts.objectFactory.spring.autoWire	name	和spring框架整合有关
			* struts.multipart			jakarta		指定文件上传用的组件
			* struts.multipart.maxSize	2097152		文件上传总文件大小限制：2M
			* struts.action.extension	action,, 	能进入struts2框架内部的url地址后缀名,多个时用逗号隔开
				* 默认是.action和没有后缀名可以进
			* struts.devMode			false		是否开启开发者模式（修改配置文件后不需要重启服务器，出错时显示更多的错误信息）
			* struts.ui.theme			xhtml		页面展示所使用的主题

			* 在struts.xml文件中，可以使用元素constant修改常量值（多个配置文件中声明了相同的常量值时，后加载的覆盖先加载的）
			* <constant name="struts.action.extension" value="action"></constant> //修改访问路径后缀名只能为.action
				* name：取常量名，value：给常量重新复值

		* Struts配置文件中的元素
			* 初级配置
			  <struts>
				<package name="user" extends="struts-default" namespace="/user">
					<action name="addUser" class="com.itheima.web.action.UserAction" method="addUser">
						<result name="success">/addUser.jsp<result>
			  </action></package></struts>
			* 使用通配符来配置
				<package name="user" extends="struts-default" namespace="/user">
					<action name="*" class="com.itheima.web.action.UserAction" method="{1}">
						<result name="success">/{1}.jsp</result>
					</action>
				</package>

			* package元素
				* 将动作类按照包的方式来管理，在配置文件中引入了面向对象的思想
				* 属性
					* name : 指定包的名称，必须写切唯一
					* extends : 指定当前包的父包，子包自动具备父包中定义的内容，面向对象思想的一种体现形式
						* 要使用到Struts2的核心功能，一般继承struts-default包
					* abstract : true / false 把当前包声明为抽象包，不能再添加action元素，用来定义公共的配置，它的作用就是用来被继承
					* namespace ： 指定当前包的名称空间，把访问路径按照模块化来管理
						* 指定了名称空间后，访问路径就变为 名称空间 + 动作名称
						* 写法为：	/名称空间
						* 默认值为： 	""
						* 一旦写了名称空间，搜索动作的顺序就变为 先找名称空间，再找动作类

			* action元素
				* 配置的是动作名称和动作类以及动作方法之间的关系
				* 属性
					* name : 指定动作名称，不能写后缀且唯一，严格区分大小写，和页面上访问的url对应
					* class: 指定动作类的全类名，给框架创建动作类实例使用的
					* method : 指定的动作方法的名称

			* result元素
				* 配置结果视图
				* 属性
					* name : 逻辑结果视图，对应的是动作方法的返回值
					* type : 以何种方式前往指定视图
						* dispatcher	转发（默认值）
						* redirect		重定向，可以重定向到jsp页面或者其他动作
						* redirectAction	重定向到另外一个动作（标签之间的字段取其他动作名称）
						* chain			转发到另外一个动作
						* stream		只针对文件下载使用

				* 配置全局结果视图
					* 同一个包下所有动作都可以使用
					* 在package元素中加入标签
						<global-results>
							<result name="" type="">XXX.jsp</result>
							...
						</global-results>
					* 当存在局部结果视图时，优先使用局部结果视图

			* 其他
				* 在struts.xml配置文件中引入其他配置文件(两个文件在相同目录下)
				* <struts> ... <include file="其它配置文件名"></include></struts>
			

		* Struts2中访问Servlet中的API
			* 使用到Struts2提供的工具类 ServletActionContext
			* 方法
				* ServletContext getServletContext()
				* HttpServletRequest getRequest()
					* HttpSession getSession()
				* HttpServletResponse getResponse()

	* Struts2中的参数封装
		* 参数封装由拦截器完成，静态 staticParams / 动态 params
		
		* 静态参数封装
			* 写在struts.xml配置文件中
			* 在action元素下使用元素param
				<param name="key">value</param>
			* 将参数封装到动作类的属性字段中，需要动作类中的属性名和name取值一致，且提供get/set方法
			
		* 动态参数封装
			* 通过表单提交获取参数
			* 属性驱动
				* 动作类和模型在一起，即动作类中提供属性和它们的set/get方法
					* 动作类中的属性名和表单中的name取值一致

				* 动作类和模型分开，即直接将数据封装到一个自定义的实体类中
					* 动作类中要有实体类对象，且需要提供相对应的get/set方法
					* 自定义实体类中要有与表单相对应的属性字段
					* 表单中的name属性取值变为 ： 自定义类的引用.属性名  自定义类的引用取自动作类中
					
			* 模型驱动
				* 动作类继承ActionSupport的同时必须实现ModelDriven接口（有泛型且为模型类）
				* 必须由我们自己实例化模型对象，即创建自定义类的对象，new ...
				* 重写ModelDriven接口中的方法，返回模型的实例化对象
				* 不再需要提供模型对象的get/set方法
				* 表单中的name取值还是和模型类中的属性名对应
				* 数据直接被封装到模型对象中
			

	* OGNL
		* 定义
			* Object-Graph Navigation Language,对象图导航语言，Struts2将其引用为自己的表达式语言，可以存取对象的任意属性，调用对象的任意方法，遍历整个对象的结构图，实现字段类型转化等功能，比EL表达式语言功能更强大

		* 使用
			* 要想使用OGNL，必须在struts2的标签中，在Jsp页面中引入Struts2标签库<%@ taglib prefix="s" uri="/struts-tags" %>
			
			* 格式：<s:property value="" />
				* value的取值是一个OGNL表达式，不是一个字符串，相当于EL表达式的${ value },默认会从OGNL的上下文中查找键为value对应的值
				* 想要将OGNL表达式变为普通的字符串，需要再在外面套上引号value="'ONGL'"

			* 普通方法的调用
				* 对象.方法名()	<s:property value="'ONGL'.length()" />

			* 静态属性的调用
				* @包名.类名@属性名称	<s:property value="@java.lang.Integer@MAX_VALUE" />
			 
			* 静态方法的调用
				* @包名.类名@方法名称
				* 必须在struts.xml文件中开启访问静态方法
				  <struts>
					<constant name="struts.ongl.allowStaticMethodAccess" value="true"></constant>	
				  </struts>

			* 使用OGNL操作List / Map
				* 在struts的表单标签中（所有标签必须提供name属性，表单默认提交方法为post），使用单选按钮标签
				* <s:radio list="{'男','女'}" name="gender" label="性别"></s:radio>
					* list属性的取值是一个ONGL表达式，把大括号内的每个元素当作list集合中的元素
					* lable属性相当于确定在单选按钮前所加的字段

				* <s:radio list="#{'male':'男','female':'女'}" name="gender" label="性别"/>
					* list的取值为ONGL表达式，#{key:value, key:value, ...}表示Map集合
					* struts2框架对应生成的html标签：
					<input type="radio" name="gender" id="gendermale" value="male"/><label for="gendermale">男</label>



		* OGNL上下文（ContextMap）
			* 动作类的生命周期
				* 动作类是多例的，每次动作访问，动作类都会实例化，所以是线程安全的，其生命周期为一次访问过程

			* 请求动作的数据存放
				* 在每次动作执行前，核心控制器StrutsPrepareAndExecuteFilter都会创建一个ActionContext和ValueStack对象，用来存储整个动作访问期间用到的数据，并且把数据绑定到当前线程局部变量上，所有是线程安全的
				* ActionContext
					* 是Struts2框架为了方便我们快速获取数据以及操作三个域和值栈所提供的一个工具类，它提供了一个ContextMap的引用

				* ValueStack
					* OGNL上下文的一部分，List集合结构，内部存入的都是对象，实现了栈的特点

			* ContextMap的组成
				* Map结构，key是一个字符串，value是一个Object对象
				* 在jsp页面查看OGNL上下文的组成部分，可以使用Struts2的一个标签: <s:debug></s:debug>
				
				* application(ServletConext)	应用域中的数据(Map结构)
				* session(HttpSession)			会话域中的数据(Map结构)
				* request(HttpServletRequest)	请求域中的数据(Map结构)
				* ValueStack					值栈（List + 栈结构）
				* 其他
					* action
					* parameters
					* attr
				
			* ContextMap中数据的存取
			
				* 三个作用域中的数据的存取



				* 值栈中数据的存取




		* Struts2框架对EL表达式的改变
			* 没有具体指定在哪查找元素时EL表达式的查找顺序
			* pageScope --> requestScope --> ValueStack --> ContextMap --> sessionScope --> applicationScope

		* Struts2的拦截器
			* struts2的重要组成部分，针对访问Action进行拦截
			* 对动作方法的一种增强方式

			* 拦截器的执行时机
				* 执行动作之前，多个拦截器正序执行
				* 执行结果视图后，倒序执行

			* 默认拦截器栈
				* defaultStack，在struts-default.xml中定义

			* 自定义拦截器
				* 自定义一个拦截器类实现Interceptor接口或继承AbstractInterceptor类（推荐），重写intercept抽象方法
					* intercept()方法
						* 有参数列表 : (ActionInvocation invocation)
						* 有返回值类型 ： String
							* 返回值 return invocation.invoke();表示放行

				* 在struts.xml文件中声明一个拦截器
					* 在package元素下定义元素
					* <interceptors> <interceptor name="" class="" /> </interceptors>
						* name 给拦截器取名，唯一  class 自定义拦截器的全类名
						
				* 在struts.xml文件中引用自定义的拦截器 	
					* 在action元素下定义元素
					* <interceptor-ref name=""></interceptor-ref>
						* name取值为在声明拦截器时给拦截器取得名称
						
					* 问题：当自己配置了一个或几个拦截器时，默认的拦截器就失效了

					* 优化
						* 声明了自定义拦截器之后，定义一个拦截器栈
						* <interceptors> 
						* 	<interceptor name="" class="" />
						* 定义一个拦截器栈
						*	 <interceptor-stack name="myDefaultStack">
						*	 	<interceptor-ref name="checkLoginInterceptor2"></interceptor-ref>
						* 将默认的拦截器栈加入其中 	
						*	 	<interceptor-ref name="defaultStack"></interceptor-ref>
						* 	 </interceptor-stack>
						* </interceptors>
						* 把新的拦截器栈设置为默认的拦截器栈
						* <default-interceptor-ref name="myDefaultStack"></default-interceptor-ref>

					* 要想自定义拦截器不拦截指定的动作方法需要在引用拦截器时注入参数
						* <interceptor-ref name="myDefaultStack">
						* 	<param name="声明的拦截器名.excludeMethods">动作方法名</param>
						* </interceptor-ref>
					