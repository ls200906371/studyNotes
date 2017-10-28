# JSP #
	java服务器端网页
	* 指令	用来设置当前JSP页面的一些属性
		* page
			* 写法	<%@ 属性名="属性值" 属性名="属性值"... %>
				* 属性
				* import	--导入java代码的包，可以出现多次，其他属性只能出现一次
				* pageEncoding	--代表当前页面的编码
				* contentType	--响应的内容采用的编码
				* pageEncoding和contentType	两者的编码可以不相同，当只给出了其中一个时，另一个默认的与给出的编码相同，都不写时，默认都为"ISO-8859-1"编码
				* language	--代表可以编写的语言，目前只有java
				* errorPage	--当页面出错时，跳转到错误处理的页面
				* 配置全局的错误跳转页面，在web.xml中加入标签
					<error-page>
						<error-code>500</error-code>
						<location>/资源地址(无需项目名)</location>
					</error-page>

		* include	代表的是页面的包含
			* 可以把一些JSP的页面整合到一起，对外显示
			* 被CSS和DIV替代
			* <%@ include file="页面地址(无项目名)" %>

		* taglib	用来导入其他的标签库(如JSTL)
			* 格式	<%# taglib prefix="" uri="" %>
				* prefix：标签的前缀名称，标签库默认带有
				* uri：引入标签的地址
				
	* 内置对象
		在JSP页面中可以直接使用的对象
		* 		引用			内置对象				常用方法
		 	 request	HttpServletRequest		域对象，获取参数，cookie
		  	response	HttpServletResponse		响应头
		 	 session	HttpSession				域对象
			application	ServletContext			域对象
			  out		PrintWriter				打印输出流，向页面输出
			pageContext	PageContext				当前页面的上下文对象
		* PageContext	域对象
			* 可以传递数据，但是域的范围仅限于当前页面
			* 可以获取到其他的内置对象
			* 可以向其他域对象
				* void setAttribute(String name,Object value,int scope)
					* 向指定域对象中存入键和值,scope为指定常量代表指定域对象
				* void setAttribute(String name,Object value)
					* 默认向最短生命周期的域对象(PageContext)中存入键和值

				* Object getAttribute(String name)	默认PageContext中查找
				* Object getAttribute(String name,int scope)	指定域对象中查找

				* Object findAttribute(String name) 全域查找，从生命周期最短的开始，依次递增

# EL表达式 #
	JSP内置的一种表达式语言
	* 目的	替换JSP中java脚本元素
	* 格式	${ 表达式语言 }
	* 作用
		* 获取域对象中的值，不存在返回""
			* ${ 域对象.属性}
			* ${ 域对象["属性"]}
		* 执行运行
	* EL内置的对象
		只有pageContext是PageContext域对象类型，其它为Map集合类型
		* pageScope		--代表当前页面
		* requestScope	--代表request范围
		* sessionScope	--代表session范围
		* applicationScope	--代表application
		* ...
		* cookie	--代表所有的Cookie对象
		* pageContext	--用于：${ pageContext.request.contextPath } --代表WEB项目的虚拟路径，即http://localhost:8080/项目名，简写为/项目名

	* EL表达式
		* + - * / %
		* ==	eq
		* !=	ne
		* <		lt
		* >		gt
		* <=	le
		* >=	ge
		* &&	and
		* !		not
		* ||	or
		* empty	判断对象是否为null或集合的长度是否为0

# JSTL #
	* JSP的标准标签库，使用需要导入jar包
	* JSTL依赖EL表达式，JSTL的标签获取值需要使用EL表达式来取值
	* 在JSP界面引入JSTL标签库
		<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

	* JSTL标签
		* <c:out></c:out>
			输出内容（通过EL表达式获取）
			* 属性
			* value	--代表输出的内容，必须出现的 <c:out value="${ 属性 }"></c:out>
			* default	--代表默认值
			* escapeXml	--设置是否转义	<c:out value="<a href='http://www.baidu.com'>点击</a>" escapeXml="false"></c:out>

		* <c:set></c:set>
			向域对象中存入值
			* 属性
				* var	--键
				* value	--值
				* scope	--作用域（取值为：page request session applicaion）

		* <c:remove></c:remove>
			从域对象中移除值
			* 属性
				* var	--键

		* <c:if></c:if>
			进行语句判断，只有if没有else
			* 属性
				* test	--必须要出现的，判断条件test="${ 属性 eq 值}"
				* var	--保存判断条件的结果
				* scope	--作用域

		* <c:choose></c:choose>
			判断，复合的标签，一起出现,等同于if else
			<c:choose>
				<c:when test="${判断表达式}"> </c:when>
				<c:when test="${判断表达式}"> </c:when>
				...
				<c:otherwise></c:otherwise>
			</c:choose>

		* <c:forEach></c:forEach>
			* 迭代数据
			* 属性
				* var	--声明变量
				* begin	--开始
				* end	--结束包含尾
				* step	--步长
				* varStatus	--在迭代过程中产生的一些信息，保存在了该变量中
					* 信息属性
					* count	--迭代次数
					* index	--下标
					* ...
				* items	--代表的是要迭代的集合或数组
				* 普通for循环
					<c:forEach var="i" begin="0" end="5" step="1" varStatus="vs">
						${ i }	-- ${vs.counts}
					</c:forEach>
				* 增强for循环
					<c:forEach var="str" items="list">
						${str}<br/>
					</c:forEach>