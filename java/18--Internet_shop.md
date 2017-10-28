# XXX商城案例 #

## 用户USER ##
	* 用户注册
		* 通用Servlet: BaseServlet
			* 继承HttpServlet,重写service()方法
			* 获得请求参数method
			* 反射获取要执行的方法的Method对象
			* 执行当前运行类对应的方法
			* 通过方法返回值确定请求转发的位置
			* 如果方法名为空，默认调用execute方法
			public void service(...,...) throws ...{
				try{
					String mName = request.getParameter("method");
					if(mName == null) {
						mName = "execute";
					}
					Method method = this.getClass().getMethod(mName,HttpServletRequest.class, HttpServletResponse.class);
					
					String path = (String) method.invoke(this,request,response);
					if(path != null) {
						request.getRequestDispatcher(path).forward(request, response);
					}
				}catch(Exception e) {
					throw new RuntimeException(e);
				}
			}
			public String execute(..., ...) throws Exception {
				return null;
			}

		* MyBeanUtils
			* 对BeanUtils进一步封装，同时处理日期转换
				public static void populate(Object bean, Map<String, String[]> map) {
					try {
						//创建BeanUtils提供的时间转换器
						DateConverter dc = new DateConverter();
					
						//设置需要转换的格式,方法参数为String数组
						dc.setPatterns(new String[] {"yyyy-MM-dd","yyyy-MM-dd HH:mm:ss"});

						//注册转化器
						ConvertUtils.register(dc, java.util.Date.class);

						//封装数据
						BeanUtils.populate(bean, map);
					}catch(Exception e) {
						throw new RuntimeException(e);
					}
				}	

		* UUIDUtils
			* 获取32 / 64长度的UUID字符串
				public static String getUUID() {
					return UUID.randomUUID().toString.replace("-", "");
				}
				public static String getUUID64() {
					return getUUID() + getUUID();
				}

		* 创建过滤器对中文编码问题处理
			public void doFilter(... req, ... resp, FilterChain chain) throws ...{
				//强转
				... requset = (...)req;
				... response = (...)resp;

				//设置编码
				request.setCharacterEncoding("UTF-8");
				response.setContentType("text/html; charset = UTF-8");

				//创建自定义MyRequest包装request
				MyRequest mr = new MyRequest(request);

				//放行
				chain.doFilter(mr, response);
			}

			* 自定义MyRequest
				//实现HttpServletRequestWrapper类，重写三个方法
				public class Myrequest extends HttpServletRequestWrapper {
					//定义引用
					private HttpServletRequest request;
					//定义构造方法接收request
					public MyRequest(HttpServletRequest request) {
						super(request);
						this.request = request;
					}

					//重写getParameterMap()方法
					public Map<String, String[]> getParameterMap() {
						try {
							//获得原始数据
							Map<String, String[]> map = request.getParameterMap();
							//判断是否是get请求，如果是,处理编码
							if("GET".equalsIgnoreCase(request.getMethod())) {
								//遍历map
								for(Map.Entry<String, String[]> entry : map.entrySet()) {
									string[] values = entry.getValue();
									//遍历所有值,处理编码
									for(String val : values) {
										if(val != null) {
											val = new String(val.getBytes("ISO-8859-1"),"UTF-8");
										}
									} 
								}
							}
							return map;
						}catch(Exception e) {
							throw new RuntimeException(e);
						}		 
 					}

					//重写getParameterValues方法，基于getParameterMap（）方法
					public String[] getParameterValues(String name) {
						//Map集合中通过键获取值get(key)
						String[] values = getParameterMap().get(name);
						return values;
					}

					//重写getParameter()方法,基于getParameterValues()方法
					public String getParameterV(String name) {
						String[] values = getParameterMap().get(name);
						if(values != null) {
							return values[0];
						}
						return null;
					}

				}

		* 发送激活邮件
			* 导入mail.jar包
			* 编写MailUtils工具类发送邮件,外网模拟
				public static void sendMail(String to, String code) {
					//创建Properties对象，存入发件人邮箱信息
					Properties prop = new Properties();
					prop.setProperty("mail.host", "smtp.??.com"); 
					prop.setProperty("mail.smtp.auth", "true");

					//将Properties对象传入session对象中
					Session session = Session.getInstance(prop, new Authenticator() {
						public PasswordAuthentication getPasswordAuthentication() {
							return new PasswordAuthentication("发件邮箱名", "密码");
						}
						
					});
					
					//将session传给Message对象
					Message message = new MimeMessage(session);
					try {
						//设置发件人
						message.setFrom(new InternetAddress("发件邮箱名"))；
						//设置收件人
						message.addRecipient(RecipientType.TO, new InternetAddress("收件邮箱名"));
						//设置主题
						message.setSubject("...");
						//设置内容
						message.setContent("..."); //内容可以使用html标签
						//使用Transport对象发送
						Transport.send(message);
					}catch(Exception e) {
						e.printStackTrace();
					}
				}

## 商品管理 ##
	* AJAX异步加载
	* 缓存技术
		* 通常指内存中的一块空间，介于应用程序和永久性数据存储源（硬盘，数据库...）之间，
		* 作用是降低应用程序直接读写永久性数据的频率，从而提高应用的运行性能
		* 常见的缓存技术
			* Memcached
			* redis
			* EhCache	一个纯Java的进程内缓存框架，具有快速、精干的特点
				* 导入ehcache的4个jar包
				* 在src目录下添加ehcache.xml的配置文件
				//创建缓存管理器
				InputStream is = 当前类.class.getClassLoader().getResourceAsStream("ehcache.xml");
				CacheManager cm =CacheManager.create(is);
				//获得分类缓存对象
				Cache cache = cm.getCache("缓存区名");
				//获得缓存内容
				Element el = cache.get("缓存内容名");
				if(el == null) {
					//缓存内容
					Object obj = ...
					//存入缓存
					cache.put(new Element("缓存内容名",obj));
				}else {
					//获取缓存内容
					Object obj = el.getObjectValue();
				}