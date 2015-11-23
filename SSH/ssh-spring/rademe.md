Spring框架
==========
* IOC
* DI  依赖注入是IOC的一种实现
* 控制翻转:翻转的是什么? 对象的管理

* IOC局限性 -> 分布式不能使用

* 注入:必须的public的成员方法

* Spring中几种注入方式? 1.基于构造器 2.基于set方法注入 3.实现特殊接口

* AOP如何实现? 
		*  Cglib<继承>
		*  jdk动态代理<接口> 
* AOP主要做什么? A aject Advice(前置,后置,环绕<例子:计算方法执行时间>) Pointcut<切点> JoinPint<连接点>
		*  控制事务
			*  pointCut
			*  advice    :传播特性<request>  :隔离级别 :回滚策略<非受控异常/运行时异常回滚 :
			*  readonly 提升性能
			*  事务开启在那一层? service why? action可以吗?可以,不好,占用时间长
		*  日志
		*  kiss思想
* Spring装配
	* 名字
	* 类型




1,简介
-------

* 1,轻量级IOC(对Bean对象的管理)和AOP(功能扩展)的容器框架

2,IOC容器创建
------------

	1-7-1) 采用BeanFactory创建
			BeanFactory是框架核心API,主要负责管理Bean对象的生命周期.功能比较单一.			
			默认情况下,单例的.
			在第一次调用getBean()时创建对象;
						
	1-7-2) 采用ApplicationContext创建(推荐)
				
	ApplicationCotnext是BeanFactory对象的子接口.功能比BeanFactory强大,
	除了可以对Bean对象进行生命周期管理,还提供了加载资源,国际化等功能;				
	默认情况下,单例的.
	在加载配置文件创建IOC容器时,就已经创建好Bean对象了.	
	如果不希望在加载配置时创建对象,可以采用延迟初始化.lazy-init="true"							
	如果采用多例创建对象,延迟初始化失效.	

3,bean对象配置
--------------
			<!-- 声明Bean对象 -->
			<bean id="loginAction" class="com.bjpowernode.web.action.LoginAction"></bean>

4,Set方法注入
--------------

		4-1) 属性为字符串类型,可以通过value之间赋值(value属性的值,默认作为字符串类型使用的)
			
			private String usercode;
			
			public void setUsercode(String usercode) {
				this.usercode = usercode;
			}
			
			<bean id="loginAction" class="com.bjpowernode.web.action.LoginAction" scope="prototype" lazy-init="true">
				<property name="usercode" value="admin"></property>
			</bean>
		
		4-2) 属性为业务对象类型;可以通过ref引用一个对象,然后通过set方法给对象属性赋值;
			private UserService userService;
			public void setUserService(UserService userService) {
				this.userService = userService;
			}
			
			<bean id="loginAction" class="com.bjpowernode.web.action.LoginAction" scope="prototype" lazy-init="true">
				<property name="userService" ref="userService"></property>
			</bean>
			
			<bean id="userService" class="com.bjpowernode.service.UserService"></bean>
		
		4-3) 属性类型为int类型
			
			private int age;
			setter
			
			<!-- 属性为int类型,那么value的值必须是能够转换为十进制整数类型的字符串; -->
			<property name="age" value="0x23"></property>
			
		4-4) 属性为字符串数组
			
			<!-- 给字符串数组赋值,可以通过逗号分隔,框架会通过逗号进行split(),然后给数组初始化
			<property name="names" value="zhangsan,lisi,wangwu"></property>	
			
			如果字符串子串中本身含有逗号,采用下面方式:
			<property name="names">
				<list>
					<value>zhang,san</value>
					<value>lisi</value>
					<value>wangwu</value>
				</list> 
			</property>
			
		4-5) 属性为List集合
			
			<property name="list">
				<list>
					<value>zhang,san</value>
					<value>lisi</value>
					<value>wangwu</value>
					<ref bean="userService"/>
				</list> 
			</property>	
			
		4-6) 属性为Map集合
				<property name="map">
					<map>
						<entry key="a" value="b"></entry>
						<entry key="b" value-ref="userService"></entry>
					</map>
				</property>	
				
		4-7) 属性为Properties集合
				
				<property name="props">
					<props>
						<prop key="username">root</prop>
						<prop key="password">123</prop>
					</props>
				</property>	
5,构造方法注入
------------
	1-1) 框架默认查找构造方法,是按照字符串类型作为构造方法参数进行查找的.	
		如果,希望按照指定类型作为构造方法的查找,那么,可以设置type属性

	1-2) 如果指定了参数类型了,那么,框架按照指定类型进行查找,如果希望通过构造方法参数的顺序来查找,那么可以设置index属性
		同时给同一个属性注入属性值,那么,setter方法注入的值将构造方法注入的值覆盖.

6,Spring框架的IOC - Bean对象的生命周期
--------------------------------------
	
		对象生命周期:
			指的是对象创建,初始化,使用,销毁的过程.
			
		1-1) 构造方法
		
			BeanFactory : 
				默认单例的,在第一次调用getBean()时反射创建对象;
			
			ApplicationContext : 
				默认单例的,在加载配置文件时,反射创建对象;
				延迟初始化:lazy-init="true"
				
			框架解析配置文件,获取完整类名称,反射调用构造方法来创建对象,然后,通过set注入或构造注入,初始化属性值;
			
		1-2) 初始化方法
			
				动态代码块:		{}....
				构造方法:			LoginAction(String usercode, int age)
				setter方法		setAge(int age)...
				初始化方法		init()...
				
			框架允许提供一个无参数的方法,用于初始化;
			
			在配置文件中指定init-method属性,来调用初始化方法.
			
		1-3) 销毁方法
			
			框架允许提供一个无参数的方法,用于对象销毁前的清理性工作(单例时,容器销毁前,会调用该方法)
			
			在配置文件中指定destroy-method属性,来调用销毁方法.		
					
				
	2) 对象创建方式:
		默认,单例的:scope="singleton"	
		如果多例:scope="prototype"		
		
		有状态的,需要多例创建;(解决线程安全问题)
		无状态的,需要单例创建;(节省内存开销,提高性能)

7,Spring框架的IOC - 自动装配
---------------------------
	
		如果采用Set方法注入和构造方法注入,属性比较多,配置比较复杂,如果希望简化配置,可以采用自动装配.
		
		框架提供autowire属性配置,可以自动查找关联对象,然后通过set方法注入或构造方法注入,实现对象之间的关联.
		
		自动装配功能不支持简单类型(基础类型,String,Date)
		
		框架提供了5种装配方式.
		
		1-1) byName(重要)
		
			创建当前对象时,框架根据属性名称,查找与属性名称一致的Bean对象的id名称,如果有一致的名称对象,那么,就将该对象注入到当前对象中.
		
			原理:还是通过setter方法实现依赖注入.
		
		1-2) byType
		
			创建当前对象时,框架根据属性类型,查找与属性类型一致的Bean对象的class名称,如果有一致的类型对象,那么,就将该对象注入到当前对象中.
		
			原理:还是通过setter方法实现依赖注入.
			
			按照类型查找Bean对象,有可能查找到多个相同类型的对象,那么注入会存在异常.
			
				解决:目标对象中定义数组或集合(泛型)类型引用,将查找到的相同类型的多个对象,都注入给目标对象;
		
		1-3) constructor
			
			原理:通过带参数的构造方法进行依赖注入;
		
		1-4) no
			
			不支持自动装配.
		
		1-5) default(重点)
			
			默认值.
			
			表示与<beans>标签的default-autowire属性的取值一致.
			因为default-autowire属性的默认值是no,所以,默认情况下,框架是不支持自动装配的.
			
			如果设置了父标签的默认自动装配方式了,子标签可以通过默认方式,与父标签采用一致的自动装配方式;
			如果,父标签与子标签都各自设置装配方式了,那么,子标签的自动装配方式起作用.

	
8, Spring框架应用在Web环境下使用
----------------------------------	
		1-1) 拷贝jar包
		
			commons-logging-1.1.3.jar
			junit3.8.1.jar
			log4j-1.2.17.jar
			spring-beans-3.2.2.RELEASE.jar
			spring-context-3.2.2.RELEASE.jar
			spring-core-3.2.2.RELEASE.jar
			spring-expression-3.2.2.RELEASE.jar		
			
			//在Web项目环境下使用Spring框架,需要引用web jar包
			spring-web-3.2.2.RELEASE.jar
			
		1-2) 拷贝Spring配置文件
			applicationContext.xml
			log4j.properties
			
		1-3) 在web.xml中需要配置一个核心监听器,框架来负责创建IOC容器
			(作用:监听ServletContext对象的创建和销毁:
				当ServletContext对象被创建了,那么框架通过监听器来创建IOC容器;
				当ServletContext对象被销毁了,那么框架通过监听器来销毁IOC容器)
				
				创建IOC容器的对象具体类型:XmlWebApplicationContext  (ClassPathXmlApplicationContext)

				监听器是Servlet技术中的一个组成部分;
				可以用于监听Web应用中对象的生命周期事件;
				
				可以监听HttpServletRequest,HttpSession,ServletContext对象的创建和销毁;
				还可以监听HttpServletRequest,HttpSession,ServletContext对象的属性值的变化(添加属性,删除属性,覆盖属性).
			
				开发一个监听器程序,需要实现特定监听器接口:
				javax.servlet.ServletContextListener		//监听ServletContext对象的创建和销毁
				 	public abstract void contextInitialized(javax.servlet.ServletContextEvent arg0); //ServletContext对象创建时,会被自动调用:执行初始化操作
				 	public abstract void contextDestroyed(javax.servlet.ServletContextEvent arg0);//ServletContext对象销毁时,会被自动调用:执行销毁操作
				javax.servlet.ServletContextAttributeListener
					public abstract void attributeAdded(javax.servlet.ServletContextAttributeEvent arg0);//往ServletContext范围添加属性值被调用
					public abstract void attributeRemoved(javax.servlet.ServletContextAttributeEvent arg0);//从ServletContext范围删除属性值时被调用
					public abstract void attributeReplaced(javax.servlet.ServletContextAttributeEvent arg0);//覆盖ServletContext范围的属性时被调用
				 
				 javax.servlet.http.HttpSessionListener  //监听HttpSession对象的创建和销毁
				 	
				 javax.servlet.http.HttpSessionAttributeListener //监听HttpSession对象属性值的变化.
				 
				 javax.servlet.ServletRequestListener	//监听HttpServletRequest对象的创建和销毁
				 
				 javax.servlet.ServletRequestAttributeListener //监听HttpServletRequest对象的属性值的变化
			
				 
			<!-- 应用上下文参数,相当于往application范围存放了一个数据
				String value = application.getInitParameter("key");
			 -->
			<context-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:applicationContext.xml</param-value>
			</context-param>
			
			<!-- 负责创建IOC容器和销毁IOC容器
				默认加载文件路径:/WEB-INF/applicationContext.xml
				如果希望修改加载文件路径:可以配置应用上下文参数
			 -->
			<listener>
				<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
			</listener> 	
			
		1-4) 获取IOC容器	(从ServletContext范围获取)
		
			WebApplicationContext ac = WebApplicationContextUtils.getWebApplicationContext(application);
			
		1-5) 获取Bean对象
			
			//从IOC容器中获取User对象
			User user = (User)ac.getBean("user");	

9 AOP面向切面编程
---------------------
* Spring框架的AOP (Aspect Oriented Programming) 面向切面编程
	
		Aspect : 是一个抽象概念.
			具体:日志服务,	异常统一处理,事务统一处理,权限统一控制等;
			
		PointCut : 切入点表达式(匹配需要执行扩展功能的连接点)
		
		JointPoint : 连接点
			在Spring中,连接点就是需要功能扩展的方法;在其他AOP编程中,连接点还可能是属性等;
			
		Advice : 通知 (用来执行功能扩展的代码)
				
			前置通知: 在连接点执行前需要执行功能扩展
			方法返回通知: 在连接点执行后需要执行功能扩展
			异常通知: 在连接点执行时出现异常了,需要执行功能扩展
			最终通知: 在连接点执行时,不管是否出现异常,都需要执行的功能扩展.
			环绕通知:	在连接点执行前和执行后都需要执行功能扩展.	
			
		Weave : 织入
			在执行连接点时动态对其进行功能扩展的过程.
			在配置文件里做的配置,静态的;但是在请求流程中动态的;	
			
		代理对象: 对于Spring框架,在获取目标对象时,动态生成代理对象:
			如果目标对象有接口:产生JDK动态代理.(基于接口)
			如果目标对象没有接口:产生Cglib动态代理.(基于继承);
		
		目标对象: 连接点所在,需要扩展的对象;

		
* 利用Spring中的AOP完成功能扩展(日志打印)
		
			1-1-1) 拷贝jar包:
				
				IOC : 
					spring-beans-3.2.2.RELEASE.jar
					spring-context-3.2.2.RELEASE.jar
					spring-core-3.2.2.RELEASE.jar
					spring-expression-3.2.2.RELEASE.jar
					
				AOP : 
					struts-2.3.20\lib\aopalliance-1.0.jar   AOP规范的jar包
					aspectjrt.jar							AspectJ 框架的jar包
					aspectjweaver.jar						AspectJ 框架的jar包
					spring-aop-3.2.2.RELEASE.jar		
					spring-aspects-3.2.2.RELEASE.jar						
		
			1-1-2) 拷贝applicationContext.xml;增加aop的命名空间和约束文件;
		
			1-1-3) 启用AOP功能,自动生成目标对象代理对象;
					<aop:aspectj-autoproxy />
		
			1-1-4) 声明目标对象和扩展对象
				
					<!-- 目标对象 -->
					<bean id="targetDAO" class="com.bjpowernode.dao.TargetDAO"></bean>
					
					<!-- 扩展对象 -->
					<bean id="log" class="com.bjpowernode.aop.Log"></bean>
					
			1-1-5) 组合目标对象和扩展对象之间关系
					<aop:config>
						<!-- 声明切面:日志扩展功能 -->
						<aop:aspect id="aspectLog" ref="log">
							<!-- 声明切入点表达式: public * *(..)  表示匹配所有public的方法(连接点) -->
							<aop:pointcut expression="execution(public * insert*(..))" id="pointcutLog"/>
							<!-- 前置通知: 在执行连接点前执行功能扩展 -->
							<aop:before method="startLog" pointcut-ref="pointcutLog"/>
							
							<!-- 最终通知: 在执行连接点时,不管是否存在异常,都将执行功能扩展 -->
							<aop:after method="endLog" pointcut-ref="pointcutLog"/>
						</aop:aspect>
					</aop:config>

	
* 利用Spring中的AOP完成功能扩展(事务处理)
		
			1-1-1) 拷贝jar包:
				
				IOC : 
					spring-beans-3.2.2.RELEASE.jar
					spring-context-3.2.2.RELEASE.jar
					spring-core-3.2.2.RELEASE.jar
					spring-expression-3.2.2.RELEASE.jar
					
				AOP : 
					struts-2.3.20\lib\aopalliance-1.0.jar   AOP规范的jar包
					aspectjrt.jar							AspectJ 框架的jar包
					aspectjweaver.jar						AspectJ 框架的jar包
					spring-aop-3.2.2.RELEASE.jar		
					spring-aspects-3.2.2.RELEASE.jar						
		
			1-1-2) 拷贝applicationContext.xml;增加aop的命名空间和约束文件;
		
			1-1-3) 启用AOP功能,自动生成目标对象代理对象;
					<aop:aspectj-autoproxy />
		
			1-1-4) 增加事务扩展类,以及扩展对象声明
					<!-- 扩展对象:对业务层Bean的方法进行事务扩展 -->
					<bean id="tx" class="com.bjpowernode.aop.TransactionAOP"></bean>
					
			1-1-5) 事务扩展配置
				
				<!-- 声明切面:事务扩展功能 -->
				<aop:aspect id="aspectTx" ref="tx" order="1">
					<!-- 声明切入点表达式: public * *(..)  表示匹配所有public的方法(连接点) -->
					<aop:pointcut expression="execution(public * com.bjpowernode.service.*.*(..))" id="pointcutTx"/>
					<!-- 前置通知: 在执行连接点前执行功能扩展 -->
					<aop:before method="startTansaction" pointcut-ref="pointcutTx"/>
					
					<!-- 方法返回通知: 在连接点执行后执行功能扩展 -->
					<aop:after-returning method="commitTansaction" pointcut-ref="pointcutTx"/>
					
					<!-- 异常通知: 在连接点执行时出现异常执行功能扩展 -->
					<aop:after-throwing method="rollbackTansaction" pointcut-ref="pointcutTx"/>
					
					<!-- 最终通知: 在执行连接点时,不管是否存在异常,都将执行功能扩展 -->
					<aop:after method="resetTansaction" pointcut-ref="pointcutTx"/>
				</aop:aspect>		

*  利用Spring的AOP增加Hibernate事务功能扩展
	
		事务边界: 一般加在业务层
			就是业务层方法开始执行了,开启事务;执行结束了,提交事务;有异常了,回滚事务;
			
		1-1) 简化DAO层代码,Session不需要自己管理;事务不允许手动编程方式增加;而是通过Spring的声明式事务给业务方法增加事务处理;	
		
			需要设置Hibernate框架的hibernate.current_session_context_class属性,值是Spring提供上下文实现类的完整名称;
		
				<!-- 一个事务对应一个Session.
					给业务层方法增加事务时,会首先打开一个Session,并且将Session与当前线程进行绑定;
					业务方法执行完成,事务提交完成了,Spring框架将Session与当前线程解除绑定,并且关闭Session;
				 -->
				<prop key="hibernate.current_session_context_class">org.springframework.orm.hibernate4.SpringSessionContext</prop>
		
			DAO层需要使用Session时,需要通过sessionFactory.getCurrentSession()来获取;
	
		1-2) 利用SPring的AOP来管理事务(声明式事务),同时也管理Session对象;
		
			1-2-1) 拷贝aop相关jar包
				
					aopalliance-1.0.jar
					aspectjrt.jar
					aspectjweaver.jar
					spring-aop-3.2.2.RELEASE.jar
					spring-aspects-3.2.2.RELEASE.jar
					
			1-2-2) 在spring配置文件中增加aop和tx的命名空间和约束文件引用;						
	
	
					<beans 
					    ...
					    xmlns:aop="http://www.springframework.org/schema/aop"
					    xmlns:tx="http://www.springframework.org/schema/tx"
					    ...
					    http://www.springframework.org/schema/aop       
					    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
					    http://www.springframework.org/schema/tx 
					    http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
					    ...>
					    
			1-2-3) 启用AOP,自动创建目标对象的代理对象
					<aop:aspectj-autoproxy />		    
	
			1-2-4) 声明事务扩展对象
			
					<!-- 事务扩展对象 -->
					<bean id="transactionManager" class="org.springframework.orm.hibernate4.HibernateTransactionManager">
						<property name="sessionFactory" ref="sessionFactory"></property>
					</bean>
					
			1-2-5) 声明扩展对象和目标对象之间关系
				
					<!-- 配置事务扩展对象与目标对象之间关系 -->
					<aop:config>
						<!-- 将通知和切入点表达式关联在一起 -->
						<aop:advisor advice-ref="adviceTx" pointcut-ref="pointcutTx"/>
					
						<aop:aspect>
							<!-- 声明切入点表达式: -->
							<aop:pointcut expression="execution(* com.bjpowernode.service.UserService.*(..))" id="pointcutTx"/>
						</aop:aspect>
					</aop:config>
					
					<!-- 
						transaction-manager : 取值,如果取值为transactionManager,那么可以省略这个属性配置,否则,不能省略;
					 -->
					<tx:advice id="adviceTx" transaction-manager="transactionManager">
						<tx:attributes>
							<!-- 
								* 	用于匹配方法名称,可以匹配多个字符
								propagation="REQUIRED" 如果方法存在一个事务,那么久加入该事务;如果方法没有事务,开启一个新的事务
								isolation="DEFAULT" 表示采用数据库默认隔离级别:MySQL 4
							 -->
							<tx:method name="query*" propagation="REQUIRED" isolation="DEFAULT"/>
							<tx:method name="update*" propagation="REQUIRED"  isolation="DEFAULT"/>
							<tx:method name="delete*" propagation="REQUIRED"  isolation="DEFAULT"/>
							
							<tx:method name="query*" propagation="REQUIRED"  isolation="DEFAULT"/>
							<tx:method name="get*" propagation="REQUIRED"  isolation="DEFAULT"/>
							<tx:method name="check*" propagation="REQUIRED"  isolation="DEFAULT"/>
						</tx:attributes>
					</tx:advice>
	
* 注意问题:
			1.切入点表达式一定要正确,否则,不会产生代理对象;
			2.通知配置的方法名称一定配置正确,否则,Spring不会开启事务,也不会打开Session;

			DAO层使用getCurrentSession()无法获取Session.抛异常;

			3.查询操作,一般配置只读read-only="true",为了提高效率;
			4.框架默认回滚策略:
			只对RuntimeExeption异常回滚,对编译期异常是不回滚的;				
			修改回滚策略:rollback-for="java.lang.Exception"			

* Spring框架解决与Hibernate框架集成时,延迟加载的有效期问题(延迟加载异常, no session异常).	
		
		+  org.hibernate.LazyInitializationException: could not initialize proxy - no Session	
	
		+ Spring管理事务,一个事务一个Session,一般业务层方法执行前,打开Session,开启事务,并且将Session与当前线程绑定;
			当业务层方法执行结果,提交事务后,Session自动关闭了;
			如果Sesssion在业务层关闭,那么延迟加载功能失效了,所以会产生延迟加载异常;
		
		1-1) Spring框架提供延迟加载异常解决方案:
				通过OpenSessionInView模式: 
						ThreadLocal<Session> + Filter/Interceptor
	
			需要配置Spring框架提供Filter:
			
				<!-- 解决Hibernate框架延迟加载异常问题 -->
				<filter>
					<filter-name>OpenSessionInViewFilter</filter-name>
					<filter-class>org.springframework.orm.hibernate4.support.OpenSessionInViewFilter</filter-class>
				</filter>
				<filter-mapping>
					<filter-name>OpenSessionInViewFilter</filter-name>
					<url-pattern>/*</url-pattern>
				</filter-mapping>
				
			注意问题:
				1. 并不是所有请求都需要执行过滤器:需要采用延迟加载功能前请求路径来需要过滤(延迟Session关闭)
				2. 一旦配置这个过滤器,那么,在请求来到过滤器时打开,回到过滤器时关闭;	
				3. 通过获取名称叫作"sessionFactory"的Bean对象,打开Session,并且与线程进行绑定(一个事务对应一个Session);
				4. 过滤器必须放置在Struts2框架核心控制器之前;放置在之后,不起作用的;
			Struts2框架执行完Action请求流程,没有执行chain.doFilter(req,resp);所有,配置在之后过滤器得不到执行;