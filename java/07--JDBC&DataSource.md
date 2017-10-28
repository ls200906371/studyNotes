

**技术分析之JDBC的回顾**
	
	1. JDBC的概述
		* Java DataBase Connectivity
		* 目的使用Java的代码来操作数据库
		* 需要使用JDBC（Java数据库的连接）规范来操作数据
	
	2. 驱动
		* 是MySQL驱动的jar包
	
	3. JDBC的开发步骤
		* 步骤一：注册驱动
		* 步骤二：获得连接
		* 步骤三：创建执行SQL语句对象
		* 步骤四：释放资源
	
	4. JDBC的核心API
		* DriverManager类
			* 加载驱动（Class.forName("com.mysql.jdbc.Driver")）
			* 获得连接
		
		* Connection接口
			* 获得执行SQL语句对象.
		        * Statement createStatement();
		        * PreparedStatement prepareStatement(String sql);
		
		* Statement接口
			* 执行SQL语句
		        * int executeUpate(String sql); 		--执行insert update delete语句
		        * ResultSet executeQuery(String sql); 	--执行select语句
			
		    * 执行批处理
		        * addBatch(String sql);
		        * clearBatch();
		        * executeBatch();
		
		* ResultSet接口
		    * 遍历结果集：next();
		    * 获得结果集中的数据.getXXX(int c); getXXX(String name);
	
----------
	
**步骤分析**
	
	1. 步骤一：创建Java项目，引入mysql的驱动包.
	2. 步骤二：编写程序
	3. 步骤三：注册驱动
	4. 步骤四：获得连接
	5. 步骤五：执行SQL
	6. 步骤六：释放资源
	
----------
	
**代码实现**
	
	0. 开发的工具选择了STS
		* 统一工作空间的编码，点击窗口 -- 选择选项 -- 搜索workspace -- 设置UTF-8（选择UTF-8）
	
	1. 创建表的SQL语句如下
		create database web07;
		use web07;
		create table t_category(
			cid int primary key auto_increment,
			cname varchar(20)
		);
	
	2. 具体的代码如下
		package com.itheima.demo1;
		import java.sql.Connection;
		import java.sql.DriverManager;
		import java.sql.PreparedStatement;
		import java.sql.SQLException;
		import org.junit.Test;
		
		public class JdbcDemo1 {
			/**
			 * 添加分类
			 */
			@Test
			public void run1(){
				Connection conn = null;
				PreparedStatement stmt = null;
				try {
					// 加载驱动
					Class.forName("com.mysql.jdbc.Driver");
					// 获取连接
					conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/web06", "root", "root");
					// 编写SQL语句
					String sql = "insert into t_category values (null,?)";
					stmt = conn.prepareStatement(sql);
					// 设置参数
					stmt.setString(1, "电子产品");
					int result = stmt.executeUpdate();
					if(result > 0){
						System.out.println("添加分类成功");
					}
				} catch (ClassNotFoundException e) {
					e.printStackTrace();
				} catch (SQLException e) {
					e.printStackTrace();
				}finally{
					if(stmt != null){
						try {
							stmt.close();
						} catch (SQLException e) {
							e.printStackTrace();
						}
					}
					if(conn != null){
						try {
							conn.close();
						} catch (SQLException e) {
							e.printStackTrace();
						}
					}
				}
			}
		}

----------
	
**总结之工具类编写**
	
	1. 编写配置文件
		* 第一步：在src的目录创建db.properties文件（key=value出现的）	
	
	2. 具体的代码实现
		package com.itheima.utils;
		import java.io.FileInputStream;
		import java.io.FileNotFoundException;
		import java.io.IOException;
		import java.sql.Connection;
		import java.sql.DriverManager;
		import java.sql.ResultSet;
		import java.sql.SQLException;
		import java.sql.Statement;
		import java.util.Properties;
		
		/**
		 * JDBC的工具类
		 * @author Administrator
		 */
		public class JdbcUtils {
			
			private static final String DRIVERCLASS;
			private static final String URL;
			private static final String USERNAME;
			private static final String PASSWORD;
			
			static{
				//  加载配置文件
				Properties pro = new Properties();
				try {
					pro.load(new FileInputStream("src/db.properties"));
				} catch (FileNotFoundException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				}
				DRIVERCLASS = pro.getProperty("driverClass");
				URL = pro.getProperty("url");
				USERNAME = pro.getProperty("username");
				PASSWORD = pro.getProperty("password");
			}
			
			/**
			 * 加载驱动
			 */
			public static void loadDriver(){
				try {
					Class.forName(DRIVERCLASS);
				} catch (ClassNotFoundException e) {
					e.printStackTrace();
				}
			}
			
			/**
			 * 获取连接
			 * @return
			 */
			public static Connection getConnection(){
				// 先加载驱动
				loadDriver();
				try {
					return DriverManager.getConnection(URL, USERNAME, PASSWORD);
				} catch (SQLException e) {
					e.printStackTrace();
				}
				return null;
			}
			
			/**
			 * 释放资源
			 * @param conn
			 * @param stmt
			 */
			public static void release(Connection conn,Statement stmt){
				if(stmt != null){
					try {
						stmt.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
				if(conn != null){
					try {
						conn.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
			}
			
			/**
			 * 是否资源
			 * @param rs
			 * @param conn
			 * @param stmt
			 */
			public static void release(ResultSet rs,Connection conn,Statement stmt){
				if(rs != null){
					try {
						rs.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
				if(stmt != null){
					try {
						stmt.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
				if(conn != null){
					try {
						conn.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
			}
		}

----------

**总结之使用工具类完成增删该查操作**

----------

### 案例二：使用连接池改造JDBC的程序 ###

----------

**需求分析**

	1. 提升程序运行的效率,采用连接池对JDBC的部分的效率进行提升

----------

**技术分析之连接池的概述**
	
	1. 什么是连接池
		* 原来：需要自己创建连接和销毁连接，这样是比较消耗时间，资源等
		* 现在：有一些连接池，已经创建好了一些连接，现在可以从连接池中获取到，这样就节省创建连接时间，直接使用这些连接，归还到连接池中。
			* 节省创建连接与释放连接，性能消耗
			* 连接池中连接起到复用的作用，提升程序性能
	
	2.  常见的开源连接池
		* DBCP连接池
		* C3P0连接池
	
----------
	
**技术分析之连接池的原理分析**
	
	1. DataSource接口，SUN公司提供的接口，任何的连接池（开源的，自定义的）必须要实现该接口
	2. DataSource接口的方法
		* Connection getConnection()
		* 开源的连接池已经实现了DataSource接口，通过getConnection()方法就能从连接池中获取到连接
	
	3. 自定义连接池代码
		* 创建连接池的时候，初始化好一些连接了
		* 用完之后，归还连接。
	
	4. 这些开源的连接池已经把Connection接口中的close()给增强了
		* 原来Connection接口中的close()是销毁连接。
		* 而现在，再使用Connection接口中的close()的时候，已经变成了归还连接的方法！！
	
	5. 增强了某个类中的某个方法？
		* 继承
		* 装饰者模式（必须要有接口的环境）
			* 增强的类与被增强的类要实现相同的接口
			* 增强的类中可以获取到被增强的类的引用
			* 是你还有你，一切拜托你！！
		
		* 动态代理（基础加强课程，框架使用动态代理）
	
----------
	
**技术分析之DBCP连接池**
	
	1. DBCP连接池，由apache组织提供开源的连接池，导入开发的jar包。
		* commons-dbcp-1.4.jar
		* commons-pool-1.5.6.jar
	
	2. 学习连接池，都有两种方式
		* 手动编码，去设置4大参数
			* 需要使用的的类：BasicDataSource，常用的方法如下
				* Connection getConnection()
				* void setDriverClassName(String driverClassName)
				* void setUrl(String url)
				* void setUsername(String username)
				* void setPassword(String password)
		
		* 通过配置文件的方式，设置4大参数，DBCP连接池支持properties属性文件的配置方式
			* 属性文件的写法
				* driverClassName=com.mysql.jdbc.Driver
				* url=jdbc:mysql:///day13
				* username=root
				* password=root
			
			* 读取配置文件
				* BasicDataSourceFactory类中，提供了一个静态方法
					* static DataSource createDataSource(Properties properties)		
	
----------
	
**技术分析之C3P0连接池**
	
	1. C3P0开源的连接池，需要导入开发的jar包
		* c3p0-0.9.1.2.jar
	
	2. 学习，只要学习该对象，不管是手动编码还是配置文件，都使用同一个类 ComboPooledDataSource
		* ComboPooledDataSource类，手动设置参数的方法入下
			* ComboPooledDataSource()
			* setDriverClass()
			* setJdbcUrl()
			* setUser()
			* setPassword()
		
		* 重点掌握配置文件的方式
			* C3P0会默认查找XML的配置文件
			* 名称是固定的：c3p0-config.xml
			* 放在src的目录下
	
	3. 以后在使用C3P0连接池，需要做哪些步骤？
		* 先导入C3P0开发的jar包
		* 再导入C3P0的配置文件（名称是固定的，存放在src的目录下）
		* 使用ComboPooledDataSource类
	
----------
	
**技术分析之修改MyJdbcUtils的工具类**
	
	1. 修改代码
	2. 以后再使用MyJdbcUtils的工具类的时候，需要引入外部的文件
		* 把C3P0的jar包导入工程中
		* 需要在src的目录下引入c3p0-config.xml的配置文件（修改数据库的名称）
		* 需要引入MyJdbcUtils的工具类

----------

### 案例三：能够使用DBUtils工具类完成CRUD的操作 ###

----------

**需求分析**

	1. 能够使用DBUtils完成CRUD的操作

----------

**技术分析之DBUtils工具类**

	1. 先导入DBUtils的开发jar包
		* commons-dbutils-1.4.jar
	
	2. DBUtils里面提供的类和方法
		* QueryRunner类		-- 完成增删改查所有的操作	
		
		* QueryRunner类提供的方法
			* QueryRunner(DataSource ds)
			* int update(String sql, Object... params)
			* <T> T query(String sql, ResultSetHandler<T> rsh, Object... params)
		
		* ResultSetHandler接口的实现类
			* BeanHandler			-- 把一条记录封装到一个对象中
			* BeanListHandler		-- 把一条记录封装到一个对象中，把对象存入到List集合中
			* MapHandler			-- 把一条记录封装到一个Map集合中
			* MapListHandler		-- 把一条记录封装到一个Map集合，把map存入到list集合中
			* ScalarHandler			-- 封装聚合函数

----------
