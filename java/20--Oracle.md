# Oracle #
	* RDBMS	关系型数据库管理系统
	* 由多个Oracle数据库和多个Oracle实例组成
		* Oracle数据库	位于硬盘上实际存放数据的文件
		* Oracle实例		位于物理内存里的数据结构，由一个共享的内存池和多个后台程序组成
	* 实例可以操作数据库，在任何时刻一个实例只能与一个数据库关联

	* sqlplus name/password@ip:1521/orcl as sysdba
	* startup 开启数据库
	* alter user userName identified by password  修改密码
	
## 查询语句 ##
	* select * | {[distinct] column, ...} from table
	* 包含空值null的表达式都为空值 in(a,b,null)可以获得结果，not in(a,b,null)不能获得结果
	* || 连接符 实现字符的连接，类似于java中的 +
	* show user	显示当前用户
	* select * from tab;	显示当前用户下的表
	* desc tbName;	显示表结构
	* show linesize	显示行宽 / set linesize n 设置行宽
	* col colName for a8 设置列宽占8个字符 / col colName for 9999 设置列宽占四个数字位
	* ed 修改命令语句		/ 执行上一条语句
	* host cls 清屏
	* concat('a','b') 拼接
	* dual	伪表，为了让某些语句符合规则 select 3+2 from dual;

	* SQL和SQL*PLUS
		* SQL
			* 一种语言
			* ANSI标准
			* 关键字不能缩写
			* 使用语句控制数据库中的表的定义信息和表中的数据

		* SQL*PLUS
			* 一种环境
			* Oracle的特性之一
			* 关键字可以缩写
			* 命令不能改变数据库中的数据的值
			* 集中运行

		* iSQL*Plus
			* 描述表结构
			* 编辑SQL语句
			* 执行SQL语句
			* 将SQL语句及其执行结果保存在文件中
			* 在保存的文件中执行语句
			* 将文本文件装入SQL*Plus编辑窗口


## 集合运算 ##
	* 并集
		* UNION 
		* UNION ALL 重复取相同部分
	* 交集
		* INTERSECT		intersect
	* 差集
		* MINUS
		* select * from A minus select * from B  / 在第一个select的结果中但不在第二个select的结果中

	* 注意事项
		* UNION / UNION ALL / INTERSECT / MINUS 关键字两端的select语句中的参数类型和个数一致
			* select a,b,c... from A UNION select d,e,f... from B / a,b,c...必须和d,e,f...一致
		* 当多个集合运算共存时，可以用()改变集合的执行顺序
		* 如果有order by字句，必须放到最后一句查询语句后
		* 集合运算采用第一个语句的表头作为查询结果的表头

## 数据处理 ##
	* DML	数据操作语言
		* 向表中插入数据
			* insert into 表名（列字段,...） values(值,...)
			* 日期型数据和字符型数据一样需要用单引号括起来 / 关键字sysdate表示当前系统时间
			* 没有给指定列赋值默认赋值为Null
			* 可以在values()中加入变量，再为变量赋值达到插入数据的目的，格式为 &变量名 ，一般为 &列名

			* 从其他表中拷贝数据 / 在insert语句中加入子查询
				* insert into tbName(a,b,c,...) select d,e,f,... from tbName2 where ...
				* a,b,c,...必须和d,e,f,...一致，包括个数和类型

		* 更新数据
			* 一般格式
				* update tbName set column = val,column2 = val,... [where ...]
			* 在update语句中加入子查询
				* update tbName set column = (select a from A where ...) where column2 = (select b from B where ...)

		* 删除数据
			* delete from tbName [where ...]
				* 当和其他表存在主外键联系时无法删除
			* Delete / Truncate
				* 都是删除表中的数据
				* Delete逐条删除 / Truncate先删除表，再创建表
				* Delete操作可以rollback,可以闪回（flashback） / Truncate不能
				* Delete操作可能产生碎片，并不释放空间 / Truncate不会，并释放空间

## 数据库事务 ##
	* 组成
		* 一个或多个DML（数据操作语言）语句
		* 一个DDL（数据定义语言）语句
		* 一个DCL(数据定义语言)语句

	* 流程
		* 以第一个DML语句的执行作为开始
		* 以下面的其中之一作为结束
			* 显示结束：commit / rollback
			* 隐式结束：自动提交	DDL语言 / DCL语言 / exit(事务正常退出)
			* 隐式回滚：系统异常终止	关闭窗口 / 死机 / 断电

	* 安全
		* 使用commit和rollback的优点
			* 确保数据的完整性
			* 数据改变被提交之前预览
			* 将逻辑相关的操作分组

	* savepoint命令
		* 在当前事务中创建保存点
		* 使用rollback to savepoint 回滚到保存点
		* savepoint update_done ... rollback to update_done
	
	* 隔离级别
		* Read uncommitted	不能解决任何读的问题
		* read committed	只能避免脏读
		* repeatable read	不能避免幻读
		* serializable		避免所有读的问题

		* 脏读：一个事务读取到了另一个事务已经更新但还没有提交的事务，若事务回滚，读取的数据无效
		* 不可重复读：一个事务读取到另一个事务更新的数据，前后结果不一致
		* 幻读：类似于不可重复读

		* Oracle支持read committed(默认) 和 serializable
		* MySQL4中都支持，默认为repeatable read

## 数据库对象 ##
	* 表		基本的数据存储集合，由行和列组成
		* DDL 创建和管理表
			* 命名规则（表名和列名）
				* 必须以字母开头
				* 必须在1-30个字符之间
				* 只能包含A-Z,a-z,0-9,_,$和#
				* 不能和其它对象重名
				* 不能使用Oracle中的保留字
				* Oracle默认存储是都存为大写
				
				* 数据库名只能是1-8位，datalink可以是128位，和其它一些特殊字符

			* 创建表所需的条件
				* 用户必须有create table 权限
				* 必须有足够的存储空间

			* 格式
				* create table tbName(column dataType [default val], ...)

			* 数据类型
				* varchar2(size)	可变字符
				* char(size)		不可变字符
				* number(size)		数值
				* date				日期（默认格式：dd-MM-yy）
				* ...
			
			* 使用子查询来创建表
				* create table tbName(column, ...) as select column, ... from tbName2 where ...
				*  子查询中的列要和指定的列一一对应
				*  通过列名和默认值定义出列

			* 修改表
				* alter table tbName add(column dataType, ...) 添加
				* alter table tbName modify(column dataType [default val], ...) 改列类型、默认值
				* alter table tbName drop column colName; 删除列
				* alter table tbName rename column old_colName to new_colName; 该列名

			* 删除表
				* drop table tbName;
				* 数据和结构都被删除
				* 所有正在运行的相关事务都被提交
				* 所有相关索引被删除
				* 删除表后不能回滚，但可以闪回

			* 清空表
				* delete / truncate table tbName;

			* 改变对象名称
				* 该表名：rename tbName to new_tbName;

			* 约束
				* not null
				* unique
				* primary key
				* foreign key
				* check
					* check(条件) 每一行记录必须满足条件

	* 视图	从表中抽出的逻辑上相关的数据集合
		* 原理
			* 视图是一种虚表
			* 视图建立在已有表的基础上，视图赖以建立的表称为基表
			* 向视图提供数据内容的语句为select语句，可以将视图理解为存储起来的select语句
			* 视图向用户提供基表数据的另一种表现形式

		* 优点
			* 限制数据访问
			* 提供数据的相互独立性
			* 简化复杂查询
			* 同样的数据可以有不同的显示方式
			* 视图不能提高性能

		* 创建格式
			* create view viewName（column,...） as select column,... from tbName where ... [with read only]; 
			* select * from viewName 查询视图

		* 使用规定
			* 当视图中包含以下元素时不能使用delete
				* 组函数
				* Group by 子句
				* distinct 关键字
				* rownum 伪列

			* 当视图中包含以下元素时不能使用update
				* 组函数
				* group by 子句
				* distinct 关键字
				* rownum 伪列
				* 列的定义为表达式

			* 当视图定义中包含以下元素时不能使用insert
				* 组函数
				* group by 子句
				* distinct 关键字
				* rownum 伪列
				* 列的定义为表达式
				* 表中非空的列在视图定义中未包括

			* 可以使用with read only 屏蔽对视图的DML操作
			* 删除视图：drop view viewName;
			
	* 序列	提供有规律的数值
		* 原理
			* 可供多个用户用来产生唯一数值的数据库对象
			* 自动提供唯一的数值
			* 共享对象
			* 主要用于提供主键值
			* 将序列值装入内存可以提高访问效率
		
		* 定义序列
			* create sequence sName [increment by n] [start with n] [maxvalue n] [minvalue n] [cycle | nocycle] [cache n];

		* 删除序列：drop sequence sName

	* 索引	提高查询的效率
		* 原理
			* 一种独立于表的模式对象，可以存储在与表不同的磁盘或表空间中
			* 索引被删除或损坏，不会对表产生影响，只会影响查询速度
			* 索引一旦建立，Oracle管理系统会对其进行自动维护
			* 在删除一个表时，所有基于该表的索引都会被删除
			* 通过指针加速Oracle服务器的查询速度
			* 通过快速定位数据的方法，减少磁盘I/O

		* 创建索引
			* 自动：在定义primary key 或unique约束后，系统自动在对应列上创建
			* 手动
				* create index iName on tbName(column, ...);
		* 删除索引
			* drop index iName;
		
		* 创建时机
			* 列中数据值分布范围很广
			* 列经常在where子句或连接条件中出现
			* 表经常被访问而且数据量很大，访问的数据大约占总数据的2%到4%


	* 同义词		给对象起别名
		* 方便访问其它用户的对象
		* 缩短对象名字的长度

		* 创建
			* create synonym sName for tbName;

		* 删除
			* Drop synonym sName;

## PL/SQL程序设计 ##
	* 原理
		* Procedure Language/SQL
		* 是Oracle对sql语言的过程化扩展
		* 在SQL命令语言中增加了过程处理语句，使SQL语言具有了过程处理能力

	* 程序结构
		* declare
			说明部分 （变量声明，光标声明，例外声明）
		  begin
			语句序列(DML语句，if,循环)
		  exception
			（例外处理声明）
		  End;
		  /

		* 要在命令行窗口输出内容，需要设置：set serveroutput on
		* 输出语句：dbms_output.put_line('hello world');

	* 说明变量
		* char / varchar2 / date / number / boolean / long
		* 给变量赋值时使用 :=	例如 var := 'a';
		* 应用型变量
			* my_name emp.ename%type	my_name的类型与emp表中ename的类型一致
		* 记录型变量
			* emp_rec emp%rowtype		emp_rec.ename:='ADAMS'

	* if语句
		* if 条件 then 执行语句1；
		  else 执行语句2；
		  end if;

		* if 条件 then 执行语句1；
		  elsif 条件 then 执行语句2；
		  ...
		  else 语句；
		  end if;

	* 循环
		* loop							例：declare
			exit [when 结束循环条件];			 n number := 1;
			执行语句；						begin
			条件控制语句；						loop
		  end loop;									exit when n > 10;
													dbms_output.put_line(n);
												end loop;
											end;
											/	(输出1到10)

		* for i in a..b					例：declare
		  loop								 i number := 1;
			执行语句；						begin 
		  end loop;								for i in 5..10
												loop
													dbms_output.put_line(i);
												end loop;
											end;
											/ (输出5到10)
		* while 执行条件						
		  loop
			执行语句；
			条件控制语句；
		  end loop;	

	* 光标
		* 定义：类似于结果集，可以用来存储多条查询结果
		* 属性
			* %isopen	是否打开
			* %rowcount	影响的行数
			* %found	光标中还有值
			* %notfound	光标中没有值
		* 语法
			* cursor 光标名[(参数名 参数类型, ...)] is select语句
				* 当光标带有参数时可以将参数应用到select语句的条件中,打开光标时给参数赋值
				* 例：cursor c1(jobc emp.job%type) is select ename,sal from emp where job=jobc;
				* 打开光标时赋值：	open c1('clerk')
				
			* 打开光标	open 光标名;
			* 取一行光标的值到指定变量中	fetch 光标名 into 指定变量,...；
				* 光标中的数据类型必须和指定变量的数据类型一致，定义时使用引用性变量
			* 关闭光标	close 光标名		（释放资源）
			* 例： declare
					cursor c1 is select ename,sal from emp;  --定义光标c1
					pename emp.ename%type;	--	定义变量接收光标中的数据
					psal emp.sal%type;
				   beging
					open c1;	--	打开光标
					loop	--循环遍历光标
						fetch c1 into pname,psal;	--取出光标中的记录到指定变量中
						exit when c1%notfound;		--当没有值时退出循环
						dbms_output.put_line(pname || '的薪水是' || psal); --打印结果
					end loop;
					close c1;		--关闭光标
				   end;
				   /	(执行全部语句)

	* 例外
		* 程序设计语言提供的一种功能，用来增强程序的健壮性和容错性
		* Oracle的异常处理
			* 系统定义的例外
				* No_data_found
				* Too_many_rows
				* Zero_Divide
				* Value_error
				* Timeout_on_resource

			* 可以在pl/sql语句中的exception语句下对异常处理
				* 例：exception
						when zero_divide then dbms_output.put_line('0不能做分母');
						when ... then ...;

			* 自定义例外
				* 在declare中定义一个例外：no_emp_found(例外名) exception
				* 在begin中抛出例外：		raise no_emp_found;
				* 在exception中捕捉处理例外：	when no_emp_found then dbms_output.put_line('没有找到员工');


	* 存储过程
		* 存储在数据库中供所有用户和程序调用的子程序加存储过程、存储函数
		* 创建语法
			* create [or replace] procedure 过程名[(参数列表)] as PL/SQL语句
			* 参数列表
				* 参数名 in 参数类型		不能作为输出，即加在select语句的条件中
				* 参数名 out 参数裂变		作为输出，加在select语句的查询结果中 select column,... into out参数变量,... from tbName
		* 存储过程的调用
			* 1
				* set serveroutput on
				* begin
				* 	过程名[(参数值)];
				* end;
				* /
			* 2
				* set serveroutput on
				* exec 过程名[(参数值)];
				
	* 存储函数
		* 存储程序，可带参数（同存储过程），并返回一计算值
		* 创建语法
			* create [or replace] function 函数名（参数列表） return 返回值类型 as PL/SQL语句

		* 调用
			* 在declare中声明变量或光标接收函数返回值
			* 在begin中使用或输出

	* 触发器
		* 是一个与表相关联的、存储的PL/SQL程序，每当一个特定的操作在指定的表上发出时，触发器被执行
		* 语句级触发器
			* 在指定的操作语句操作之前或之后执行一次是触发，不管它影响了多少行

		* 行级触发器
			* 触发语句作用的每一条记录都被触发，在航迹触发器值使用 :old.指定列 和 :new.指定列 来表示指定列中不同状态下的值

		* 创建语法
			* create [or replace] trigger 触发器名 before/after 
			  delete/insert/update[of列名] on 表名 [for each row [where ...]]
			  PL/SQL语句