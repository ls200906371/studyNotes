## Maven ##
	* 定义
		* 基于项目对象模型（POM），可以通过一小段信息来管理项目的项目管理工具
		* 开源跨平台性，主要服务于基于Java平台的项目构建，依赖管理和项目管理信息

	* 主要功能
		* 项目构建
		* 依赖管理

	* 概念模型
		* 项目对象模型（Project Object Model）
			* 通过pom.xml定义项目的坐标、项目依赖、项目信息、插件坐标等

		* 依赖管理系统（Dependency Management System）
			* 通过定义项目所依赖组件的坐标由maven进行依赖管理

		* 一个项目生命周期（Project Lifecycle）
			* Maven将项目生命周期抽象统一为：清理、初始化、编译、测试、报告、打包、部署、站点生成等

		* 一组标准集合
			* Maven将整个项目管理过程定义一组标准

		* 插件（plugin）目标
			* Maven管理项目生命周期过程都是基于插件完成

	* maven的仓库
		* 用来统一存储所有Maven共享构建的位置
		* 本地仓库
			* 用来存储从远程仓库或中央仓库下载的插件和jar包的本地文件夹，项目使用插件或jar包时，优先从本地仓库查找

		* 远程仓库
			* 私服
				* 中央仓库的代理仓库，架设在局域网内的仓库

			* 中央仓库
				* 在maven环境内部内置了一个远程仓库，服务于整个互联网，当没有私服时，本地仓库没有的jar包默认从中央仓库下载

	* Maven命令（可以多个命令组合）
		* mvn clean		删除字节码生成目录
		* mvn compile	编译
		* mvn test		测试（测试代码不会被打包或部署）
		* mvn package	打包
		* mvn install	部署（将包安装至本地仓库，让其他项目依赖）
		* 运行任一阶段的时候，它前面的所有阶段都会被执行
		
	* 坐标
		* 用来定位一个唯一确定的jar包
		* 组成
			* groupId	定义当前Maven组织名称
			* artifactId	定义实际项目名称
			* version	定义当前项目的当前版本
			* packaging	定义当前项目的打包方式
				* jar	默认
				* war
				* pom

	* 依赖管理
		* 依赖声明
		* <dependencies>
		* 	<dependency>
		* 		<groupId> </groupId>		//jar包坐标
		* 		<artifactId> </artifactId>
		* 		<version> </version>
		* 		<scope> </scope>	//定义依赖范围
		* 	</dependency>
		* </dependencies>

	* 依赖范围（scope）
		* compile	主代码classpath / 测试代码classpath / 运行时classpath都有效
		* test		测试代码classpath有效
		* provided	主代码classpath / 测试代码classpath有效
		* runtime	运行时classpath有效

	* 依赖传递
		* A依赖B,B依赖C,C是A的传递依赖
		* 取决于依赖范围

	* 依赖冲突
		* 如果直接依赖与间接依赖中包含有同一个坐标不同版本的资源依赖，以直接依赖为主（就近原则）
		* 如果直接依赖中包含有同一个坐标不同版本的资源依赖，以配置顺序下方的版本为准（就近原则）

	* 可选依赖
		* 在依赖<dependency>中添加<optional>决定此依赖是否向下传递
		* <optional>false</optional>	向下传递（默认）
		* <optional>true</optional>		不向下传递

	* 排除依赖
		* 排除多余不使用的依赖资源
		* 在依赖<dependency>中使用<exclusions>
		* <exclusions>
		* 	<exclusion>	//	可以接多个
		* 		<groupId> </groupId>
		* 		<artifactId> </artifactId>
		* 		<version> </version>	//省略时排除所有版本
		* 	<exclusion>
		* <esclusions>

	* 生命周期
		* 构建工程的步骤，对所有的构建过程进行抽象和统一
		* 三大生命周期（相互独立）
			* Clean Lifecycle	在进行真正的构建之前进行一些清理工作
			* Default Lifecycle	构建的核心部分，编译，测试，打包，部署等
			* Site Lifecycle	生成项目报告，站点，发布站点

	* Maven插件
		* 编译插件
			* <build>
			* 	<plugins>
			* <!--JDK插件-->
			* 		<plugin>
			* 			<groupId>org.apache.plugins</groupId>
			* 			<artifactId>maven-complier-plugin</artifactId>
			* 			<configuration>
			* 				<source>1.7</source>
			* 				<target>1.7</target>
			* 				<encoding>UTF-8</encoding>
			* 			</configuration>
			* 		</plugin>
			* <!--Tomcat插件-->
			* 		<plugin>
			* 			<groupId>org.apache.tomcat.maven</groupId>
			* 			<artifactId>tomcat7-maven-plugin</artifactId>
			* 			<configuration>
			* 				<port>8080</port>
			* 				<path>/<path>
			* 			</configuration>
			* 		</plugin>
			* 	</plugins>
			* </build>
			* 启动Tomcat: 	在run as Build中输入 tomcat7:run	
			
	* 继承
		* 为了消除重复，把相同的配置提取出来
		* 父工程的打包方式选择 pom（当一个工程有子工程时，其打包方式必须为pom）
		* 子工程继承父工程
			* <parent>
			* 	<groupId>	//父工程坐标
			* 	<artifactId>
			* 	<version>
			* </parent> 
		* 在父工程中对jar包进行依赖，在子工程中都会继承此依赖
		
		* 父工程的插件子工程也会继承
		
		* 父工程中统一管理版本号
			* Maven使用<dependencyManagement>来管理依赖的版本号
				* 只是定义了依赖jar包的版本号，不是实际依赖，如果子工程要依赖jar包，还是要使用<dependency>,可以省略<version>  

	* 聚合
		* 原理
			* 将一个工程拆分成多个模块开发，每个模块是一个独立的工程，但是在运行时必须把所有模块聚合到一起才是一个完整的工程，
			* 使用到继承
				