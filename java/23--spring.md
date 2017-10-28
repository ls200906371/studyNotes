# Spring #
	* 原理
		* 分层的Java SE/EE应用full-stack轻量级开源框架，以IoC和AOP为内核。

	* IoC
		* Inversion of Control	控制反转
		* 一个重要的面向对象编程的法则来消减计算机程序的耦合问题，一般分为两种类型，依赖注入（DI）和依赖查找

		* spring容器
			* 用于存放对象的容器，Map结构
.
		* Spring中的工厂
			* BeanFactory	提供的是延迟加载的思想，实际使用到bean的时候，spring容器才去创建
			* Resource resource = new ClassPathResource("bean.xml");
			* BeanFactory factory = new XmlBeanFactory(resource);	初始化spring容器

		* ApplicationContext
			* 提供的是立即加载的思想，spring容器初始化的时候，bean类就被立即创建
			* 该接口中没有close方法，其实现类中有
			* 实现类
				* ClassPathXmlApplicationContext
					* 加载类根路径下的配置文件
				* FileSystemXmlApplicationContext
					* 加载本地磁盘下Spring的配置文件
	
		* 基于XML的IoC配置方式
			* 在类的根路径下创建配置文件bean.xml



			* Spring实例化bean的方式
				* <bean id="" class="bean类的全类名"></bean>
					* 把bean类交给Spring来管理
					* 使用反射的方式new了一个bean类的对象，调用的是bean类的默认无参构造函数

			* bean的作用范围
				* <bean id="" class="" scope=""></bean> scope属性的取值即表示bean的作用范围
				* singleton		单例（默认）
				* prototype		多例
				* request		应用在Web项目中，将对象存入到request域中
				* session		应用在Web项目中，将对象存入session域中
				
			* bean的生命周期
				* 在<bean>标签中使用属性init-method / destroy-method可以定义bean类的初始化方法和销毁方法
					* <bean id="" class="" init-method="init" destroy-method=""> bean类中要有init / destroy方法 
				* 当bean对象是单例时，Spring容器销毁时（close()），bean类也销毁
				* 当bean对象不是单例时，Spring就不在管理bean类的销毁了，销毁Spring容器时不会触发destroy方法
				* 随着使用时创建该对象，当长时间不用时，由java的垃圾回收机制回收


			* 依赖注入（DI）
				* 向Spring容器中注入参数，即给Spring容器中的实例对象中的属性字段赋值
				
				* 使用构造函数注入（#必需要有对应的有参构造函数#）
					* 标签<construction-arg>
						* 属性
							* index	根据字段在构造函数中出现的索引注入，从0开始，加双引号
							* name	根据字段名称注入
							* type	根据字段类型注入

							* ref	引用其它的bean赋值
							* value	要注入的值，只能是基本类型和String类型

				* set方法注入(#要注入的引用必须有对应的set方法#)
					* 标签 <property name="" ref=""/value="">
						* 属性
							* name	要注入的属性名称
							* ref	注入引用bean类型的值
							* value	注入基本数据类型或String类型的值

				* 集合数组的注入(#必须提供对应的set方法#)
					* 只要集合结构相同，元素可以互换
					* 注入数组
						* <property name="数组引用名">
						* 	<array>
						* 		<value>AAA</value>
						* 		...
						* 	</array>
						* </property>
					
					* 注入List
						* <property name="">
						* 	<list>
						* 		<value>BBB</value>
						* 		...
						* 	</list>
						* </property>

					* 注入Set
						* <property name="">
						* 	<set>
						* 		<value>CCC</value>
						* 		...
						* 	<set>
						* </property>

					* 注入Map
						* <property name="">
						* 	<map>
						* 		<entry>		//key=DDD value=123
						* 			<key><value>DDD</value></key>
						* 			<value>123</value>	
						* 		<entry>
						* 		...
						* 		
						* 		<!-----------或者--------------->
						* 		
						* 		<entry key="" value=""></entry>
						* 		...
						* 	<map>
						* </property>

					* 注入properties
						* <property name="">
						* 	<props>
						* 		<prop key="">AAA</prop>
						* 		...
						* 	</props>
						* </property>
							

		* Spring的分配置文件开发


		* 基于注解的IoC配置


		* Spring的IoC常用的注解
			* @Component	将当前类交个spring容器来管理
				* @Controller	表现层
				* @Service		业务层
				* @Repository	持久层

			* @Value		注入基本数据类型
			* @Autowired	按类型自动注入
			* @Resource(name=id)	按id属性值注入

	* AOP
		* Aspect orient Programming		面向切面编程  OOP(面向对象编程)的扩展
		* 通过预编译方式和运行期动态代理实现程序功能的统一维护
		* 利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低
		* 不修改源码的情况下，对程序进行增强

		* Spring中AOP的基本概念
			* Joinpoint
				* 连接点：指类中的核心业务方法，Spring只支持方法类型的连接点
			
			* Pointcut
				* 切入点：指我们对其进行功能增强的连接点
				
			* Advice
				* 通知/增强：指那些具有公共代码的类

				* 前置通知：在切入点方法执行之前实施增强
				* 后置通知：在切入点方法执行之后实施增强
				* 异常通知：在切入点抛出异常后实施增强
				* 最终通知：无论切入点方法如何执行，都在最后实施增强

				* 环绕通知：Spring提供给我们，让我们自己决定通知何时织入业务核心方法
				
			* Weaving
				* 织入，把增强应用到目标对象来创建新的代理对象的过程，spring采用动态代理织入	

			* Aspect
				* 切面
				
		* Spring中使用AOP需要明确的
			* spring框架监控切入点方法的执行，一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类型，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行
		

		* 基于XML的AOP配置
			* 导入AOP相关的jar包，由于aop是基于Ioc的，所有也必须导入IOC的jar包
			* 引入aop的名称空间

			* 配置切面
				* <aop:config>
					* 定义切面
					* <aop:aspect ref="">	ref:指定的通知对象的引用,即将通知对象交给spring来管理时对应的id属性值(<bean id="" class=""></bean>)
 						* 配置通知(前置)
 						* <aop:before method="methodName" pointcut="execution(* 包名.*.*(..))"/>
 							* method:指定通知对象中的哪个方法是通知方法
							* pointcut:指定的哪些接入点将变成切入点
								* 表达式：execution([修饰符] 返回值类型 包名.类名.方法名.(参数))
								* 使用通配符的缩写：execution(* 包名.*.*(..)) 某个包下的所有类的所有方法
							* pointcut-ref:引用定义好的通用切入点

			* 配置通用切入点
				* <aop:config>
				* 	<aop:pointcut expession="execution(* 包名.*.*(..))" id="">

		* 基于注解的AOP配置
			* 开启Spring对AOP注解的支持	
				* <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

			* 常用注解
				* @Aspect: 将当前类看成一个切面

				* @pointcut("execution(...)"): 定义切入点表达式，公共的，作用于方法上

				* 通知类型注解
					* @Before（"execution(...)"）
					* @AfterReturning（"execution(...)"）
					* @AfterThrowing（"execution(...)"）
					* @After（"execution(...)"）
					* @Around（"execution(...)"）
					* 当切入点表达式所指定的切入点方法执行时，被注解的通知方法执行


	* Spring中的JDBC模板
		* 拷贝jar包
			* Spring的IoC相关的Jar包
			* 数据库的驱动包
			* spring-jdbc-x.x.x.RELEASE,jar
			* spring-orm-x.x.x.RELEASE.jar
			* spring-tx-x.x.x.RELEASE.jar
		
		* HibernateTemplate		用于通过HibernateTemplate框架访问数据库用的


		* JdbcTemplate	用于直接通过JDBC访问数据用的
		
			* 最基本的使用
				* 创建Spring中的内置数据源
					* DriverManagerDataSource ds = new DriverManagerDataSource();
					
				* 配置链接数据库的基本信息
					* ds.setDriverClassName()
					* ds.setUrl()
					* ds.setUsername()
					* ds.setPassword()

				* 创建Spring提供的JdbcTemplate对象
					* JdbcTemplate jt = new JdbcTemplate();

				* 把数据源传给JdbcTemplate
					* jt.setDataSource(ds);

				* 执行CRUD操作
					* jt.execute("sql语句")

			* 基于Spring配置的使用
				* 拷贝要使用的jar包
				* 创建.xml的配置文件，引入beans的schema约束
					* 将JdbcTemplate交给Spring容器来管理
						* <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
					* 向JdbcTemplate中注入数据源（Spring框架自带的或者其它的,选其一即可）
						* 	<property name="dataSource" ref="driverManagerDataSource"></property>
						* </bean>

					* 配置数据源
						* Spring内置的数据源（推荐）
							* <bean id="driverManagerDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
							* 给数据源注入参数
							*	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
							*	<property name="url" value="jdbc:mysql://localhost:3306/databaseName"></property>
							*	<property name="username" value="root"></property>
							*	<property name="password" value="123"></property>
							* </bean>

						* DBCP数据源（需要导入DBCP相对应的jar包）
							* <bean id="dbcpDataSource" class="org.apache.commons.dbcp.BasicDataSource">
							* 给数据源注入参数
							*	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
							*	<property name="url" value="jdbc:mysql://localhost:3306/databaseName"></property>
							*	<property name="username" value="root"></property>
							*	<property name="password" value="123"></property>
							* </bean>

						* C3P0数据源（需要导入C3P0相对应的jar包）
							* <bean id="c3p0DataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
							* 给数据源注入参数
							*	<property name="driverClass" value="com.mysql.jdbc.Driver"></property>
							*	<property name="jdbcUrl" value="jdbc:mysql://localhost:3306/databaseName"></property>
							*	<property name="user" value="root"></property>
							*	<property name="password" value="123"></property>
							* </bean>

					* 自定义测试类，定义主方法
						* 加载配置文件
							* ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
						* 获取JdbcTemplate对象
							* JdbcTemplate jt = ac.getBean("jdbcTemplate");
						* 调用JdbcTemplate中的方法，与数据库交互
							* jt.execute("sql")  / update("sql", [Object...params]) / query("sql", [Object...params])


		* Spring中的Dao工具类
		
			* 加入Dao之后的JdbcTemplate的使用
				* 原始版(可以使用注解)
					* 在Dao的实体类中声明一个JdbcTemplate的引用，并提供相对应的set方法（在配置文件中为其注入实体对象时需要）
						* 定义操作数据库的方法，可以使用预编译处理，jdbcTemplate.update("sql(?, ...?)", Object...params)
					* 在.xml配置文件中
						* 把Dao交给Sping容器管理
							* <bean id="xxx" class="Dao实体类全类名">
						* 把JdbcTemplate注入到Dao中
							*	<property name="dataSource" ref="driverManagerDataSource"></property>
						* 配置Spring的内置数据源
							* 见上
					* 定义测试类加载.xml文件，获取Dao对象，调用其方法（需要传入参数时传参）
	
				* 改进版（不能使用注解）
					* Dao实体类在实现Dao接口的同时继承类JdbcDaoSupport
						* 类JdbcDaoSupport中声明了一个JdbcTemplate的引用，并提供了相对应的get/set方法
						* Dao实体类中就不在需要定义JdbcTemplate，要使用它时，直接掉用自身的getJdbcTemplate方法可获取
					* 在.xml配置文件中
						* 同原始版


	* Spring的事务管理
		* JavaEE体系进行分层开发，事务处理位于业务层，Spring提供了分层设计业务层的事务处理解决方案

		* Spring事务管理的三个主要接口
			* 事务管理器	PlatformTransactionManager
				* 方法
					* TransactionStatus getTransaction(TransactionDefinition definition) 获取事务状态信息
					* void commit(TransactionStatus status) 提交事务
					* void rollback(TransactionStatus status) 回滚事务

				* 实现类
					* DataSourceTransactionManager	使用Spring JDBC或iBatis进行持久化数据时使用
					* HibernateTransactionManager	使用Hibernate进行持久化数据时使用
					* ...

			* 事务定义信息	TransactionDefinition
				* 方法
					* String getName()	获取事务对象名称
					* int getlsolationLevel()	获取事务隔离级别
					* int getPropagationBehavior()	获取事务传播行为
					* int getTimeout()	获取事务超时时间
					* boolen isReadOnly()	获取事务是否只读
				* 隔离级别
					* ISOLATION_DEFAULT		默认级别（由数据库类型决定）
					* ISOLATION_READ_UNCOMMITTED
					* ISOLATION_READ_COMMITTED
					* ISOLATION_REPEATABLE_READ
					* ISOLATION_SERIALIZABLE	
				* 事务的传播行为
					* 一个方法调用了另一个具有事务的方法，事务的处理
					* REQUIRED(增删改)	
						* 如果当前没有事务，就新建一个事务，如果已经存在一个事务，加入到这个事务中

					* SUPPORTS（查询）
						* 支持当前事务，如果当前没有事务，就以非事务方式处理


			* 事务具体运行状态	TransactionStatus
				* void flush() 刷新事务
				* boolean hasSavepoint()	获取是否存在保存点
				* boolean isCompleted()		获取事务是否完成
				* boolean isRollbackOnly()	获取事务是否回滚
				* boolean isNewTransaction()获取事务是否为新的事务

			* 声明事物管理
				* 基于XML文件的配置
					* 配置事物管理器
						* <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
						* 注入数据源
						* 	<property name="dataSource" ref="driverManagerDataSource"></property>
						* </bean>
					* 配置通知
						* <tx:advice id="txAdvice" transaction-manager="transactionManager">
						* <tx:attributes>
						* 	<tx:method name="*"/>
						* 	<tx:method name="find*" read-only="true"/>
						* </tx:attributes></tx:advice>
						* 元素tx:method：
							* 属性
							* name	指定要加入事物的方法，可以使用通配符，也可以部分通赔
							* isolation	事物的隔离级别，默认为default，看数据库的隔离级别
							* rollback-for	用于配置一个异常，当发生该异常时回滚事物
							* no-rollback-for	用于配置一个异常，当发生该异常时不会滚
							* propagation	指定事物的传播行为，默认值是REQUIRED
							* read-only		指定事物是否为只读事物，默认值为false
							* timeout		指定超时时间，默认值-1，以秒为单位

					* 配置切面