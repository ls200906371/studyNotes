# XML #
	可扩展性标记语言，不同于HTML的显示数据，主要用来传输数据
	* 作用： 作为配置文件  /  传输数据
	
	* 语法： 
		* 文档声明（必须写上）
			* <?xml version="1.0" encoding="UTF-8" ?>
			* 必须出现在第一行第一列
		* 标签
			* 又开始必须有结束，<book></book> / <book/>
			* 标签区分大小写
		* 属性
			* 需用 双/单 引号扩起来
			* 一个标签上不能出现相同名称的属性
		* 注释
			* <!--注释不能嵌套-->
		* CDATA区
			* <![CDATA[内容]]> 内容不解析
			
	* XML的解析 
		* DOM解析
			* 加载整个XML文档到内存中，形成树装结构
			* 可以完成增删改查所有的的操作
			* 如果文档非常大，会产生内存溢出的问题
		* SAX解析
			* 边读边解析
			* 只能查询，不能增删改
			* 不会出现内存溢出的问题解析
			
	* XML解析工具之DOM4J
		* 导包，查API
		* 常用方法
			* SAXReader : Document read(String id)
			* Document :  Element getRootElement();
			* Element :   
				* Element element(String markupName) 
				* list<Element> elements() 
				* String getText()
				* String attributeValue(String attName)
	* XPATH
		辅助DOM4J查找的语言
		* Node ： Document和Element的父接口
			* Node selectSingleNode(String xpath)
			* List<Node> selectNodes(String xpath)

	* XML约束
		规定XML中可以出现的标签和属性
		* DTD约束
			* 创建dtd文件
			* 编写dtd文件 <!ELEMENT 元素名 元素类型>
			* 在XML文档中引入DAT文档 <!DOCTYPE 根节点名 SYSTEM "DTD文件地址">

			* 直接在XML文档中输出DTD的内容 <!DOCTYPE 根节点名 [DTD约束的内容]>

			* 引入网络中的DTD文件 <!DOCTYPE 根节点名 PUBLIC "DTD文件名" "DTD网络地址">
			
		* DTD语法
			* 元素类型 (#PCDATA)  / EMPTY /ANY || + : 1-n / * : 0-n / ? : 0-1
			* 定义属性
				* <！ATTLIST 元素名称 
						属性名 属性类型 属性约束	
						属性名 属性类型 属性约束
						...
				  >
				* 属性类型 CDATA / 枚举(值1 | 值2) / ID
				* 属性约束 #REQUIRED 必须出现的 / #IMPLIED 可选择的 / #FIXED "值"  固定值
				
		* schema约束
			* DTD不符合XML的语法结构，schema符合XML的语法结构
			* DTD的约束扩展性较差，XML文档只能引入一个DTD文件。schema可以引入多个文件
			* DTD不支持名称空间(包结构),schema支持
			* DTD支持的数据比较少，schema支持更多的数据类型
			
			* 创建schema文档的后缀名为.xsd
			* 创建schema文档的根节点为<schema></schema>，不能改变

			* 定义schema的约束文档
				* 先引入W3C的名称空间  在根节点中使用属性 xmlns = "http://www.w3.org/2001/XMLSchema"
				* ...
			* 在XML中引入schema的约束文档
				* ...

# WEB　#
	网页，javaWEB：使用java语言来编写网页
	* 静态的WEB资源
		页面数据是不变化的，浏览器可以直接访问 HTML/CSS/JS
	* 动态的WEB资源
		页面的数据是时刻在变化的，浏览器不能直接访问 Servlet / JSP
	* WEB资源结构
		* C/S   client / server  客户端 / 服务端
		* B/S   browser / server 浏览器 / 服务端
	* 开发的都是B / S架构的项目
		基于请求和响应的模式的开发
	* 服务器
		本身就是一台计算机，安装了一些指定的软件
			
				