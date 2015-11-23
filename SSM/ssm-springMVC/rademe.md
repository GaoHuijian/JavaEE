SpringMVC
==========
1, 导入jar包

2, 配置文件

web.xml
	org.springframework.web.servlet.DispatcherServlet
	DispatcherServlet 前端控制器
* 具体配置
	 <servlet>
	  	<servlet-name>springmvc</servlet-name>
	  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	  </servlet>
	  <servlet-mapping>
	  	<servlet-name>springmvc</servlet-name>
	  	<url-pattern>/</url-pattern>   
	  </servlet-mapping>

* xxx-servlet.xml   xxx为web.xml里display-name的名字
	<bean class="" name="" id=""></bean>

	id:唯一标记,要求全局应用中唯一,一个bean标签只有唯一的一个id值,id值中不能有特殊符号.<数字字母下划线,数字不能放在开头>
	name:别名,要求别名全局唯一,一个bean标签可以有若干个别名,可以用特殊符号,




控制器 MVC中的C
--------------
* 架构基础是 org.springframework.mvc.Controller 接口方法是:handleRequest()

实现Controller方法:
------------------
	* 实现Controller接口
		示例:
		<bean id="testControl" class="com.bjpowernode.springmvc.controller.TestController" />

	* 实现抽象子类:AbstractController 只需实现handelRequestInternal方法
		示例:
		<bean id="userControl" class="com.bjpowernode.springmvc.controller.UserController">
			<property name="supportedMethods">
				<set>
				<value>POST</value>
				</set>
			</property>
		</bean>

	* 继承:MultiActionController
		配置示例:
		<bean id="stuControl" class="com.bjpowernode.springmvc.controller.StudentController">
			<property name="methodNameResolver">
				<bean  class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
					<property name="mappings">
						<props>
							<prop key="/stu/login">login</prop>
							<prop key="/stu/reg">reg</prop>
							<prop key="/stu/menu/reg">reg</prop>
						</props>
					</property>
				</bean>
			</property>
		</bean>
		

命令控制器 
---------


处理器映射 handler mapping <映射请求路径和处理器之间的关系>
-------------------------
* 内置处理器映射策略:SimpleUrlHandlerMapping
	具体配置:
	<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping" >

		//访问 url controller 映射map
		<property name="urlMap">
			<map>
				<entry key="/test" value-ref="testController"></entry>
			</map>
		</property>

		//拦截器列表
		<property name="interceptors">
			<list>
				<ref bean="interceptor1"/>
				<ref bean="interceptor2"/>
			</list>
		</property>

	</bean>

* BeanNameUrlHandlerMapping



处理适配器 HandlerAdapter
------------------------
* org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter



拦截器 handerInterceptor
--------------------------
* 继承HandlerInterceptorAdapter类

视图解析器 view resolver ModelAndView mvc中的mv
--------------------------------------------------
* UrlBasedViewResolver
	* InternalResourceViewResolver 内部资源解析器,进行页面的forward,可以解析redirect
		配置实例:
		<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="viewClass" value="org.springframework.web.servlet.view.JstlView">
			</property>
			<property name="prefix" value="/WEB-INF/jsp/"></property>
			<property name="suffix" value=".jsp"></property>
		</bean>

* BeanNameViewResolve
		配置://根据Bean标签ID属性进行解析
		<bean class="org.springframework.web.servlet.BeanNameViewResolve"/>
		
		//JSTL标签解析标准视图
		<bean id="ModeAndView.viewName" class="org.springframework.web.servlet.view.JstlView">
			<property name="url" value="/WEB-INF/xx.jsp"/>//映射结果路径
		</bean> 

		//重定向视图
		<bean id="" class="org.springframework.web.servlet.view.RedirectView">
			<property name="url" value="http://www.baidu.com"/>
		</bean> 
* ResourceBundleViewResolve 独立资源视图解析器
		配置
		<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolve"/>

		独立配置文件 默认名字是:views.properties 文件是通过classpath读取的.
		### viewName.(class)=ViewClassName ###
		### viewName.url=path ###			绝对路径以协议开头,相对路径以应用名开头.
		hello.(class)=org.springframework.web.servlet.view.JstlView
		hello.url=/WEB-INF/jsp/hello.jsp

注解
----
*   配置启用注解
	<context:annotation-config/>
*   设定扫描注解位置
	<context:component-scan base-package="com.bjpowernode.springmvc.*" />
*   适配器更改为:
	<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter" />
		
	示例:
	@Controller
	@RequestMapping("/xxx")	//命名空间
	**********************************
	@RequestMapping("/xx")
	**********************************
	@RequestMapping(value="/method",method={RequestMethod.POST}) //只能是POST请求
	****************************************************************************
	@RequestMapping(value="/method",params={"xxx","yyy"}) //必须传参数
	****************************************************************************
	//接收请求参数,接收请求表单数据
	//方法的参数名与请求中的参数必须一致
	@RequestParm(require=true)
	@RequestParm(require=true ,defaultValue="xx")
####################################实例#############################################
		@RequestMapping("/hello")
		public String hello(){
		//相当于ModelAndView.viewName   /WEB-INF/jsp/hello.jsp
		return "hello";
		}
####################################实例#############################################
	/*
	 * 通配符 : * 只能匹配字符.不能匹配目录
	 */
	@RequestMapping("/test*/**")
	public String test(){
		return  "test";
	}
-------------------------------------------------------------------------------------
总结	
	方法上注解:
	@RequestMapping(value="xxxx", 					访问路径/可*/*匹配
					method={RequestMethod.POST},	限定请求方式
					params={"username", "userpswd"}	限定请求参数
					)
	参数上注解:
		参数上没注解:								方法的参数名与请求中的参数名一致.可以自动的接收请求参数
		@RequestParam("username")String name			

* SpringMVC没有所谓的域驱动,在处理请求参数注入的时候,先检索服务方法的参数名,如果有方法参数名与请求参数名一致的,进行数据注入,如果服务方法参数类型为自定义类型,检索自定义类型中的属性(property)与请求参数名是否一致,如果一致进行注入,请求参数处理时,只要有符合要求的服务方法参数,全部注入

AJAX访问服务器:
-------------	
	JSON : JavaScript Object Notation 特点格式的js对象
	格式:	{"":xx,"":yy}
	数组:
	第三方jar包依赖:
		jackson-core-asl-1.9.9.jar
		jackson-mapper-asl-1.9.9.jar
	配置:
	<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
		<property name="messageConverters">
			<list>
				<bean class="org.springframework.http.converter.json.MappingJacksonHttpMessage>
					<>
				</bean>
			</list>	
		</property>
	</bean>

------------------------------------------------------------------------------------------------------------------
页面传值:
	路径传参:/path/param1/param2/param3   路径传参必须完全匹配
		@RequestMapping("/test/{username}/{userpswd}")
		public String test(@PathVariable("username") String username,@PathVariable("userpswd")String userpswd){
			return ;
		}
	变量作用域传值
		使用ServletApi传值
		使用数据模型传值			Model
		使用Map传值,类似数据模式	Map
	配置文件上传解析器:
	<bean id="multipartResolver" 
		class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
文件上传:
		/*	<form action="xxx/xx" enctype="multipart/form-data" method="POST"></form>
		*	jar包
		*	文件上传解析器:
		*	服务方法参数必须由@RequestParam注解
		*/	服务方法参数类型必须使用MultipartFile
	@RequestMapping("/fileupload")
	public String testFileupload(@RequestParam("uploadfile") MultipartFile upload){
		return "hello";
	}
		
