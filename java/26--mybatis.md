## mybatis ##
	* 原理
		* 源自apache的iBatis,持久层框架
		* 通过xml或注解的方式将要执行的statement（JDBC处理器）配置起来
		* 并通过Java对象和statement中的sql映射生成最终执行的sql语句
		* 最后由mybatis框架执行sql语句并将结果映射成Java对象放回

	* 架构
		* SqlMapConfig.xml
			* mybatis的全局配置文件，配置了mybatis的运行环境等信息

		* XXXMapper.xml
			* sql映射文件，配置了操作数据库的sql语句，在全局配置文件中加载

		* SqlSessionFactory
			* 回话工厂，创建sql回话，通过方法openSession()
			* 由SqlSessionFactoryBuilder创建，创建完成后就不在需要它了，一般在方法内部使用
			* 整个运行期间都需要应用，可以重复使用，通常以单利模式管理

		* SqlSession
			* 回话接口，操作数据库（通过操作执行器）
			* 封装了对数据库的操作
			* 不能共享使用，线程不安全的，使用完后关闭

		* Executor
			* 执行器接口，操作数据库，包括基本执行器和缓存执行器

		* Mapped Statement
			* mybatis底层封装对象，对sql执行输入参数进行定义，对sql执行后结果进行定义
			* 映射文件中的一个sql语句对应一个该对象

	* mybatis与hibernate的不同
		* hibernate是一个完全的对象关系映射（ORM）框架,sql语句由框架生成，映射文件中配置的是java类与数据库表的映射
		* mybatis不是一个完全的ORM框架，sql语句由程序员自身编写，映射文件中配置的sql语句

	* 工程搭建
		* 导入mybatis的jar包，log4j的jar包，数据库的驱动包,
		
		* 在classpath下创建log4j.properties，mybatis默认使用log4j作为输出日志信息
			* # Global logging configuration
			* log4j.rootLogger=DEBUG, stdout
			* # Console output...
			* log4j.appender.stdout=org.apache.log4j.ConsoleAppender
			* log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
			* log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

		* 在classpath下创建SqlMapConfig.xml
			* 元素（标签）
				* properties	(属性)
					* 可以引用java属性文件中的配置信息
					* 例
						* <properties resource="db.properties"/>
						  <environments default="development">
							<environment id="development">
								<transactionManager type="JDBC"/>
								<dataSource type="POOLED">
									<property name="driver" value="${jdbc.driver}"/>
									<property name="url" value="${jdbc.url}"/>
									<property name="username" value="${jdbc.username}"/>
									<property name="password" value="${jdbc.password}"/>
								</dataSource>
							</environment>
						  </environments></properties>
						* classpath下db.properties
							* jdbc.driver=com.mysql.jdbc.Driver
							* jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8
							* jdbc.username=root
							* jdbc.password=root


				* typeAliases	（类型别名）
					* mybatis支持的别名
						* 当映射的类型为基础数据类型，别名为：_类型名（int --> _int）
						* 当映射类型为基础类型的包装类型，String,Date,Map,BigDecimal时，别名为类型名首字母小写

					* 自定义别名
						* <typeAliases>
						 	<!-- 单个别名定义 -->
						  	<typeAlias alias="user" type="com.mybatis.po.User"/>
						  	<!-- 批量别名定义，扫描整个包下的类，别名为类名（首字母大写或小写都可以） -->
						 	 <package name="com.mybatis.po"/>
					    * </typeAliases>

				* mappers		（映射器）
					* <mappers>
					*    	<mapper resource="" />
					* 属性及取值
							* resource  映射文件相对于类路径的位置
							* class		指定mapper接口全类名，要求接口名和映射文件名相同，且在同一目录下
							* name 		指定mapper接口所在包名，要求同上
					* </mappers>
			* 例
				* <?xml version="1.0" encoding="UTF-8" ?>
				* <!DOCTYPE configuration
				* PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
				* "http://mybatis.org/dtd/mybatis-3-config.dtd">
				* <configuration>
				*	 <!-- 和spring整合后 environments配置将废除-->
				*	 <environments default="development">
				*		 <environment id="development">
				*		 <!-- 使用jdbc事务管理-->
				*		 	 <transactionManager type="JDBC" />
				*		 <!-- 数据库连接池-->
				*			 <dataSource type="POOLED">
				*				 <property name="driver" value="com.mysql.jdbc.Driver" />
				*				 <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
				*				 <property name="username" value="root" />
				*				 <property name="password" value="root" />
				*			 </dataSource>
				*		 </environment>
				*	 </environments>
				*	 <!-- 加载映射文件 -->
				*    <mappers>
				*    	<mapper resource="" />
				*    	...
				*	 </mappers>
				* </configuration>

		* 创建和数据库中表格所对应的javabean类，简称Po类/pojo类

		* 创建映射文件po类名.xml
			* 元素
				* mapper	定义映射sql
					* 属性
						* namespace: 命名空间，用于隔离sql语句
					* 子元素
						* resultMap
							* 如果sql查询字段名和pojo类的属性名不一致，可以通过resultMap将字段和属性名作一个对应关系 
							* <resultMap id="" type="">		type: 要将查询结果映射到的pojo对象
							* 	<id column="" property=""/>		id:表示查询结果集的唯一标示（主键）
							* 	<result column="" property="" />	column:sql查询出来的字段名；property: 对应pojo对象的属性
							* 	
							* </resultMap>
							
						* select 查询
							* 动态sql
						





						* insert 添加
							* 子元素
							* selectKey ： mysql自增主键返回（在插入记录时不指定主键，由数据库自动生成，在插入提交后能在插入对象中获取到id值）
								* 属性
									* keyProperty: 指定返回的主键存储在pojo类中的哪个属性
									* order：selectKey的执行顺序，相对于sql语句来说，AFTER / BEFORE
									* resultType : 返回的主键类型
									* LAST_INSERT_ID() :mysql函数，返回新增的自增主键ID值
							* 例
								* <insert id="insertUser" parameterType="cn.itcast.mybatis.po.User">
								*	<!-- selectKey将主键返回，需要再返回 -->
								*	<selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
								*		select LAST_INSERT_ID()
								*	</selectKey>
								*    insert into user(username,birthday,sex,address)
								*    values(#{username},#{birthday},#{sex},#{address});
								* </insert>

						* update 修改
						* delete 删除
						* 属性
							* id: 指定唯一sql，使用sqlSession对象查询时，方法参数之一为：namespace值.id值
							* parameterType: 定义输入到sql中的映射类型，支持别名
								* mybatis通过ognl从输入对象中获取参数值拼接在sql中
							* resultType: 定义结果类型，支持别名
								* mybatis将sql查询结果的一行记录数据映射为resultType指定类型的对象
							* resultMap
								* 指定一个resultMap的id
								
						* 元素文本，即sql语句
							* #{}	表示一个占位符，可以接收简单类型或po属性值，括号中可以是value也可以是po属性名，当parameterType为复杂对象时，可以使用:属性名.属性名...
							* ${}	表示拼接sql串，可以接收简单类型值和po属性值,如果parameterType为简单类型，括号内固定为value,当parameterType为复杂对象时，可以使用:属性名.属性名...
			* 例
				* <?xml version="1.0" encoding="UTF-8" ?>
				* <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
				* "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
				* <mapper namespace="test">
				* 	<!-- 添加sql语句 -->
				*	<select id="findUserById" parameterType="int" resultType="com.mybatis.po.User">
				*		select * from user where id = #{id} 
				*	</select>
				*	<select id="findUserByName" parameterType="int" resultType="com.mybatis.po.User">
				*		select * from user where name like '%${value}%' 
				*	</select>
				* </mapper>

		* Mapper动态代理方式（替换Dao，官方推荐）
			* 原理
				* Mapper接口开发方法只需要程序员编写Mapper接口（相当于Dao接口），由Mybatis框架根据接口定义创建接口的动态代理对象，代理对象的方法体同Dao接口实现类方法
			
			* 规范
				*	Mapper.xml文件中的namespace与mapper接口的类路径相同。
				*	Mapper接口方法名和Mapper.xml中定义的每个statement的id相同 
				*	Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同
				*	Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同



