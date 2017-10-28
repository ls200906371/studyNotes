# Hibernate #
	* 定义：
		* 一个开放源代码的对象关系映射框架，对JDBC进行了轻量级的封装，全自动的orm框架，可以自动生成SQL语句，自动执行
		* 轻量级：使用方便、功能丰富
		* 框架（framework）：基本概念上的结构，用于解决或处理复杂的问题
		* ORM(对象关系映射)：将java对象映射到关系型数据库表上，通过操作java对象，就可以完成数据库表的操作（目的）

	* 执行流程
		* 编写hibernate配置文件及pojo类（简单的java对象）
			* 主配置文件
				* 在类的根路径下创建一个名称为hibernate.cfg.xml的配置文件
				* 引入DTD约束
					<!DOCTYPE hibernate-configuration PUBLIC
						"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
						"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
				* 根目录
					<hibernate-configuration>
				* 配置Session工厂，负责生产Session对象
					<session-factoy>
					* 连接数据库的基本信息
						<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
						<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/crm_hibernate_ee27</property>
						<property name="hibernate.connection.username">root</property>
						<property name="hibernate.connection.password">1234</property>
						* 配置数据库方言
						<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>

					* Hibernate的基本配置
						* 把session绑定到当前线程上
							<property name="hibernate.current_session_context_class">thread</property>
						* 配置c3p0数据源的提供商
							<property name="hibernate.connection.provider_class">org.hibernate.connection.C3P0ConnectionProvider</property>
						 
						* 打印生成的SQL语句
							<property name="hibernate.show_sql">true</property>
						* 格式化SQL语句
							<property name="hibernate.format_sql">true</property>
						* 采用何种方式生成DDL语句
							* #hibernate.hbm2ddl.auto create-drop	每次执行应用时创建，程序退出时删除
							* #hibernate.hbm2ddl.auto create	每次执行时创建新的
							* #hibernate.hbm2ddl.auto update	每次执行时验证表结构的变化，如果有变化就更新表结构
							* #hibernate.hbm2ddl.auto validate	校验表结构的变化
							* #hibernate.hbm2ddl.auto none		什么都不做
							<property name="hibernate.hbm2ddl.auto">update</property>

					* 映射文件的配置
						* 有多少个映射文件，就需要在此处配置多少行
						<mapping resource="com/itheima/domain/CstCustomer.hbm.xml" />
						</session-factory>

			* 编写映射文件
				* <!-- 搭建hibernate的开发环境
					1、拷贝必要的jar包到工作空间
					2、在实体类所在的包下创建一个名称为CstCustomer.hbm.xml的文件。 命名规则是：实体类名称.hbm.xml
  					3、为映射文件导入约束
					4、配置实体类和数据库表的对应关系
					5、配置实体类中的属性和表中的字段的对应关系
				  -->
				<!DOCTYPE hibernate-mapping PUBLIC 
   						"-//Hibernate/Hibernate Mapping DTD 3.0//EN"
   						"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
				<hibernate-mapping package="com.itheima.domain">
				<!-- class元素：
					作用：用于指定实体类和表的对应关系
					属性：
						name：用于指定实体类的全名称
						table：用于指定数据库表的名称
				-->
				<class name="CstCustomer" table="cst_customer" lazy="true">
				<!-- 配置属性：主键
					id元素：
						作用：用于配置主键
						属性：
							name：用于指定实体类中的get/set方法的名称。
							column：用于指定表中的列的名称
							type：用于指定字段的数据类型。可以不写。
				-->
				<id name="custId" column="cust_id">
					<!--用于指定主键的生成策略 。
						native：使用本地数据库的自动增长能力。
					-->
					<generator class="native"></generator>
				</id>
				<!-- 配置属性：其他字段 
					property元素：
						作用：用于映射实体类中其他字段和表中列的对应关系
						属性：
							name：用于指定实体类中的get/set方法的名称。
							column：用于指定表中的列的名称
							type：用于指定字段的数据类型。可以不写。
				-->
				<property name="custName" column="cust_name" ></property>
						......
		
				<!-- 一对多关系映射 
					set元素：
						作用：映射集合属性
						属性：
							name：指定集合属性的名称
							table：指定对方（多的一方）表的名称
					key元素
						作用：指定外键
						属性：
							column：指定对方表中外键字段的名称
					one-to-many
						作用：描述当前实体和映射集合中的元素之间的关系是一对多
						属性：
							class：指定的是对方的实体类名称
				-->
				<set name="linkmans" table="cst_linkman" lazy="false" fetch="join">
					<key column="LKM_CUST_ID"></key>
					<one-to-many class="CstLinkMan"/>
				</set>

				<!-- 多对一关系映射：多个客户对应一个来源 -->
				<many-to-one name="baseDict" class="CstBaseDict" column="cust_source"></many-to-one>
				</class>
				</hibernate-mapping>
						
		* 应用程序调用hibernate的api加载hibernate配置文件、映射文件等
		* 通过configuration创建sessionFactory会话工厂
		* 通过SessionFactory会话工厂创建session会话
		* 通过session操作数据库，session重定义了很多操作数据库的方法
		* 对于增删改数据库操作需要transaction事务管理对象提交或回滚事务
		* 对于查询数据库操作通过query对象执行查询操作
		* 释放资源，一个操作结束后关闭session，应用程序结束后关闭sessionFactory

	* API
		* Configuration
			* 用来加载配置文件，默认只会加载类的根路径下的配置文件
			* configure():加载XML配置文件
			* addClass(Class calzz):手动指定类的字节码文件
			* addResource(String path)：手动指定映射文件的位置
			* buildSessionFactory():创建Session工厂对象

		* SessionFactory
			* 用来生产Session对象，线程安全的
			* Session openSession(): 每次打开一个新的Session对象
			* Session getCurrentSession():从当前线程获取一个Session对象

		* Session
			* 对数据库进行增删改查的操作，开启事务，线程不安全的
			* save(Object obj): 保存对象
			* update(Object obj):更新对象
			* delete(Object obj): 删除对象
			* get(Class clazz,Object obj) / load(): 根据主键查询
			* Query createQuery(String HQL): 通过HQL语句查询
			* Transaction beginTransaction() : 开启事务
		
		* Transaction
			* 针对事务的控制
			* commit()
			* rollback()

		* Query
			* 执行HQL语句
			* List list()

	* 对象唯一标识符
		* 原理
			* hibernate中的OID，实例对象的主键
			* 用来区分两个对象是不是同一个，数据库中通过主键来判断两条数据是否是同一条

		* 主键生成策略
			* 定义hbm.xml映射文件和pojo类时都需要定义主键
			* 自然主键
				* 具有业务含义的字段，如学号
			* 代理主键
				* 不具有业务含义的字段。如mysql自增主键，uuid生成的字符串序列（企业中开发使用）
			
			* <id name="" column=""><generator class=" 主键生成器 "></generator></id>
			* 主键生成器
				* 代理主键
					* increment: 由hibernate维护的一个变量，每次生成主键时自动递增
						* 问题：如果有多个应用访问一个数据库，会出现主键冲突
					* identity： 由底层数据库生成标识符，前提是数据库支持自动增长数据类型
						* mysql支持，Oracle不支持
					* sequence： Hibernate根据底层数据库序列生成标识符，前提是数据库支持序列
						* Oracle支持
					* native： 根据底层数据库自动来选择identity sequence hilo

					* uuid: Hibernate采用UUID算法生成唯一标识符

					* hilo: 使用高低位算法生成主键，必须是long int short类型
						* 只能在一个数据库中保证唯一

				* 自然主键
					* assigned: 由java程序生成标识符

	* 缓存
		* 将数据库或硬盘中的数据，存入到内存中，当下次使用的时候可以从内存中直接使用，减少数据库访问次数
		* Hibernate框架中共有两个缓存，Session级别的缓存是属于一级缓存，SessionFactory级别的缓存属于二级缓存

		* 一级缓存
			* 生命周期与Session一致
			* 当Session的save方法持久化一个对象时，该对象同时被载入缓存

		* 快照机制
			* 通过Session获取到一个持久化对象时，Hibernate会将该对象载入缓存，同时生成一个不可更新的快照
			* 从缓存中获取该对象，修改该对象属性，再提交事务时，执行快照机制
			* 用缓存和快照进行比较，如果不一致，使用缓存同步数据库中的数据，同时刷新缓存和快照

	* Hibernate框架中的对象状态
		* 临时状态
			* 没有OID，没有和Session建立关系，直接new出来的对象
		* 持久化状态
			* 有OID，和Session建立了关系，通过session对象中的save(),persist(),saveOrUpdate()merge()方法等
			* 在有事务时，save()等同于persist(),没有事务时，save()默认开启hibernate自带事务（只有在创建一个新的Session对象时），persist（）不会
		* 脱管状态
			* 有OID和Session没有关系
		* 删除状态

	* Hibernate的检索方式
		* OID检索
			* get(Class clazz,String oid)/load(Class clazz,String oid)
		
		* 对象导航检索
			* 通过OID查询目标对象所关联的对象，从关联对象中获取目标对象
		* HQL检索
			* 最基本的HQL查询
				* Query query = session.createQuery("from CstCustomer");
			* 带有条件的HQL查询： 使用?作为参数占位符，索引从0开始
				* Query query = session.createQuery("from CstCustomer where columnName=?");
				* query.setString(0, "value");
			* 具名查询： 给参数占位符一个名称，格式为 :name ,赋值时省略冒号
				* Query query = session.createQuery("from CstCustomer where columnName=:columnName");
				* query.setString("columnName","value");
			* 排序查询
				* Query query = s.createQuery("from CstCustomer order by columnName desc");
				* desc 降序 / asc 升序（默认）
			* 分页查询
				* Query query = s.createQuery("from CstCustomer ");
				* query.setFirstResult(start);
				* query.setMaxResults(pageSize);  //limit start,pageSize
			* 投影查询
				* 只查询指定字段，需要实体类中提供了对应的字段的构造函数
				* Query query = s.createQuery("select new com.itheima.domain.CstCustomer(custId,custName) from CstCustomer");
		* QBC检索
			* 是一种更加面向对象的查询方式，QBC能实现的，HQL都能实现，反之亦然
			* 基本查询
				* 使用Session对象获取Criteria对象,参数是要查询的实体类的字节码
				  Criteria criteria = s.createCriteria(CstCustomer.class);
				  criteria对象也有一个list方法: List<T> list = criteria.list();
			* 带条件查询
				* Criteria criteria = s.createCriteria(CstCustomer.class);
				  criteria.add(Restrictions.eq("custIndustry", "IT培训"));//添加条件
				  criteria.add(Restrictions.eq("baseDict.dictId", "7"));//客户来源ID
				  List<T> list = criteria.list();
			* 分页查询
				* Criteria criteria = s.createCriteria(CstCustomer.class);
				* criteria.setFirstResult(0);
				* criteria.setMaxResults(2);
				* List<CstCustomer> cs = criteria.list();

		* SQL检索
			* 只有在较复杂的条件查询情况下，才可能用到
			* 获取原生SQL对象,参数为原生的SQL语句
			  SQLQuery sqlQuery = s.createSQLQuery("select * from cst_customer"); 
			* 把返回的结果通过添加实体类字节码的方法，封装到实体类中
			  sqlQuery.addEntity(CstCustomer.class);
			* 调用list()方法获取结果集合
			  List list = sqlQuery.list();

	* Hibernate的抓取策略（提升性能的手段）
		* 类级别检索策略
			* 只涉及当前类的字段数据，不涉及关联对象的数据
			* 查询的时机
				* 立即加载
					* 不管用不用，只要查询一个实体，就把当前实体中所有字段查询出来
					* 对应Session中的方法：get(Class<T> clazz,Object column);
				* 延迟加载
					* 如果只用当前实体的OID,则不发起查询，只有当用到其他字段时，才查询
					* 对应方法：load();默认是延迟加载，可以在映射配置文件中配置改为立即加载
					* 在class标签上添加属性lazy,默认值为true,改为false后就是立即加载
		
		* 关联级别检索策略