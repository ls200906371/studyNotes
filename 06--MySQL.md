# 关系型数据库 #
	MySQL / Oracle

# 登录
	* net start mysql
	* mysql -u root -p123 
	* alter database mydb character set gbk
	* net stop mysql
	* show databases 
	* use test 
	* show tables
	
## SQL ##
	结构化查询语言，用来操作关系型数据库
	* DDL
		* create table td(IP int primary key auto_increment,Name varchar(20) unique,age int not null)
		
		* alter table td add primary key(IP)  / alter table td drop primary key
		* alter table td change IP IP int auto_increment / alter table change IP IP int
	* DML 
		* delete from td where id = 1
		* update td set age = 2 where id =1
		* insert into td(id,age) values(1,2)

	* DCL
		* create user 用户名@IP地址 identified by '密码'
		* grant all on mydb.* to 用户名（含@IP地址）
		* revoke all on mydb.* from 用户名
		* drop user 用户名
	* DQL
		* select * from td where ... group by 列 having ... order by 列 asc/desc ,列 asc/desc ...

	* 聚合函数
		* count() / max() / min() / sum() / avg()
	* 单表约束
		* primary key (auto_increment)
		* unique
		* not null
		
	* 外键约束
		保证表结构中数据的完整性
		* create table td(IP int primary key auto_increment,Name varchar(20), ...)
		* create table td1(tid int primary key auto_increment,Name char(10),...fk int, foreign key(fk) references td(IP))	
		* alter table td add foreign key(fk) references td(IP)	
	
	* 多表查询
		* 内连接
			* 普通：select * from td (as) a inner join td1 (as) b ON a.IP = b.fk;
			* 隐式：select * from td,td1 where td.IP = td1.fk;
		* 外连接
			* 左外连接：select * from td left (outer) join td1 ON td.IP = td1.fk;
			* 右外连接：select * from td right (outer) join td1 ON td.IP = td1.fk;
	* 子查询
		select嵌套
		