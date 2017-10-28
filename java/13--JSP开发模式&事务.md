# JavaWeb开发模式 #

## JavaBean ##
	* 概念：一个java类，有特殊的要求
	* 要求：
		* 属性必须是私有的
		* 私有的属性必须提供get或set方法
		* 必须提供无参的构造方法
		* 实现Serializable接口(可序列化--可选)
	* 作用：用来封装数据
	* 其属性由get或set方法来决定

## JSP开发模式 ##
	* Model1
		* JSP + JavaBean的开发模式
		* 适合做一些小型的应用，简单快捷，对开发人员要求不高
		* 所有的操作都在JSP页面中，不利于维护

	* Model2
		* JSP + Servlet + JavaBean
		* servlet负责处理用户的请求，JSP负责数据显示，JavaBean负责封装数据，各个模块之间层次清晰
		* 维护方便，有利于组件的重用，适合开发复杂的应用
		* 项目开发难度大，对开发人员的要求随之提高

	* MVC模式
		* model层，即模型层，用来维护数据以及提供数据访问方法
		* view层，即视图层，通常由JSP充当，用于展示模型的部分数据或所有数据的可视化视图
		* controller层，即控制层，用于处理请求

## JavaEE开发的三层结构 ##
	* JavaEE：企业级应用技术，一款软件或是企业里用的一套处理方案

	* 三层结构(基于MVC)
		* WEB层(WEB)
			* 和客户端做交互，使用的框架有：Struts / SpringMVC
		* 业务层(service)
			* 编写业务逻辑，使用的框架有：Spring
		* 持久层（DAO）
			* 操作数据库，使用的框架有：Hibernate / MyBatis

## 事务 ##
	* 概述
		* 事务是数据库提供的一个特性
		* 组成事务的各个执行单元，要么都成功，要么都不成功

	* 在MySQL中操作事物
		* 在命令行窗口
			* start transcation;	--开启事务
			* commit;	--提供事务（事务结束，数据永久保存到数据库中）
			* rollback;	--回滚事务（事务结束，书局回滚到初始化状态）

		* MySQL数据库的事务是默认提交的，一条语句使用一个事务
			* set autocommit = off或0	--设置MySQL事务不默认提交，需手动提价

	* 在JDBC中操作事务
		* Connection接口中提供了方法
			* void setAutoCommit(boolean autoCommit)	--传入false,设置MySQL数据库的事务不默认提交
			* void commit()	--提交事务
			* void rollback()	--回滚事务

		* 事务的特性（由数据库提供）
			* 原子性		--强调事务的不可分割性
			* 一致性		--强调的是事务执行前后数据需要保证一致
			* 隔离性		--强调的是多个事务同时操作一条记录时，事务之间不能互相干扰
			* 持久性		--强调的是事务一旦结束，数据将永久的保存到数据库中

		* 不考虑事务的隔离性会引发的问题
			* 脏读：一个事务读取到另一个事务未提交的数据
			* 不可重复：一个事务读取到了另一个事务提交的数据，导致多次读取的结果不一致（强调的是update）
			* 虚读（幻读）：多次读取的数据不一致，强调的是insert

		* 设置事务的隔离级别（设置数据库的隔离级别，解决上述读的问题）
			* Read uncommitted		--什么都解决不了
			* Read committed		--只能避免脏读
			* Repeatable read		--避免脏读和不可重复，虚读有可能产生
			* Serializable			--三种全避免
			* 安全性依次递增，效率依次降低

			* 在命令行窗口中
				* select @@tx_isolation;	--查询隔离级别
				* set session transcation isolation levei --;	设置隔离级别

			* 事务添加在JavaEE的业务层中

	* DBUtils使用事务
		* QueryRunner
			* 创建QueryRunner类对象时不再传入连接池对象
			* 使用其方法
				* update(Connection con,String sql,Object...params)
				* query(Connection con,String sql,ResultSetHandle<T> rsh,Object...params)