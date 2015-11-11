Struts2 与 Hibernate集成
========================
	直接集成即可.

Spring 与 Struts2集成
=======================			
	
			Action对象谁来管理?
			
				有两种集成方案:
					第一种:Action对象依然是Struts2自己来管理;ObjectFactory
					第二种:Action对象委托给Spring来管理;BeanFactory
		
			只需要拷贝一个插件jar包:struts2-spring-plugin-2.3.20.jar
				(Struts2框架提供)实现Action与Service对象之间关联.
		
			原理: 第一种方案
				
				Struts2框架原来工厂:com.opensymphony.xwork2.ObjectFactory
				插件中提供新工厂:org.apache.struts2.spring.StrutsSpringObjectFactory
				
				StrutsSpringObjectFactory extends SpringObjectFactory
				SpringObjectFactory extends ObjectFactory
				
				框架提供插件jar包,通过新工厂代替原来工厂来创建Action,然后,通过Struts2框架来指定自动装配方式,
				通过调用Spring框架的自动装配功能,来实现Action与Service对象关系组合;
				
				if (appContext.containsBean(beanName)) {
		            o = appContext.getBean(beanName);
		        } else { //第一种集成方案
		            Class beanClazz = getClassInstance(beanName); //Struts2框架获取Action的类
		            o = buildBean(beanClazz, extraContext); //自己来创建Action对象;但是,实质调用Spring的自动装配工厂来完成Action创建及属性装配;
		        }
		        
		        Struts2框架默认设置装配方式为name  -> 相当于Spring的byName ;  在框架底层通过AutowireCapableBeanFactory.AUTOWIRE_BY_NAME
		       	 如果希望修改装配方式为type -> 相当于Spring的byType ; 在框架底层通过AutowireCapableBeanFactory.AUTOWIRE_BY_TYPE
					<constant name="struts.objectFactory.spring.autoWire" value="type"></constant>


Spring与Hibernate集成
========================
	将Hibernate框架核心对象SessionFactory委托给Spring管理

					hibernate.cfg.xml文件内容全部交给Spring进行统一配置管理;					
					<bean id="sessionFactory" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
						<property name="dataSource" ref="dataSource"></property>
						<property name="hibernateProperties">
							<props>
								<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
								<prop key="hibernate.show_sql">true</prop>
								<!-- 一个事务对应一个Session. 
								<prop key="hibernate.current_session_context_class">org.springframework.orm.hibernate4.SpringSessionContext</prop>
								-->
								<!-- Hibernate自己管理Session,每个线程绑定一个Session对象,使用后自动关闭 -->
								<prop key="hibernate.current_session_context_class">thread</prop>											
							</props>
						</property>
						<property name="mappingDirectoryLocations">
							<list>
								<value>classpath:com/bjpowernode/bean</value>
							</list>
						</property>
					</bean>
					
			1-4-2) 将SessionFactory注入给DAO对象;


Spring Struts2 Hibernate集成
===============================
	
		Struts2: 负责MVC流程
		Spring: 负责Bean对象生命周期管理
		Hibernate : 负责持久化
	
		1-1) 将Struts2框架集成到项目中
		
			1-1-1) 拷贝常用jar包:
					参考:struts-2.3.20\apps\struts2-blank\WEB-INF\lib\*.jar
			
					asm-5.0.2.jar			字节码增强工具类库:可以在JVM运行时,在内存中动态生成类,或修改已有类字节码.
					asm-commons-5.0.2.jar
					asm-tree-5.0.2.jar
					commons-fileupload-1.3.1.jar	Apache提供文件上传组件
					commons-io-2.2.jar				Apache提供文件上传组件
					commons-lang3-3.2.jar			是java.lang包的扩展程序
					freemarker-2.3.19.jar			是一个视图技术
					javassist-3.11.0.GA.jar			字节码增强工具类库(JBoss),可以在JVM运行时,在内存中动态生成类,或修改已有类字节码.
					log4j-1.2.17.jar				日志输出控制组件.可以控制日志输出级别,目标地,格式(src/log4j.properties)
					ognl-3.0.6.jar					对象图导航语言,类似于EL表达式语言,用于操作数据
					struts2-core-2.3.20.jar	Struts2框架核心类库
					xwork-core-2.3.20.jar	WebWork框架核心类库
		
			1-1-2) 拷贝struts.xml文件
				
					参考:struts-default.xml文件
				
			1-1-3) 在web.xml文件中增加核心控制器配置
				
					<!-- 核心控制器:负责请求转发Action -->
					<filter>
						<filter-name>struts2</filter-name>
						<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
					</filter>
					<filter-mapping>
						<filter-name>struts2</filter-name>
						<url-pattern>/*</url-pattern>
					</filter-mapping>	
		
			1-1-4) 完成登录验证功能
			
					a.建立相关页面:
							login.jsp    
							success.jsp
					b.创建User.java
					c.创建LoginAction.java
					d.完成Action的配置

		1-2) 将Spring框架集成到项目中
		
			1-2-1) 拷贝常用jar包
					commons-logging-1.1.3.jar
					junit3.8.1.jar
					log4j-1.2.17.jar
					spring-beans-3.2.2.RELEASE.jar
					spring-context-3.2.2.RELEASE.jar
					spring-core-3.2.2.RELEASE.jar
					spring-expression-3.2.2.RELEASE.jar
					spring-web-3.2.2.RELEASE.jar				
			
			1-2-2) 拷贝applicationContext.xml文件
					applicationContext.xml
					log4j.properties
					
			1-2-3) 配置核心监听器来创建IOC容器
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
			
			1-2-4) 建立业务层接口和实现类:
					UserService.java
					UserServiceImpl.java
					
					业务层bean对象,交给Spring来管理的:
						<bean id="userService" class="com.bjpowernode.service.UserServiceImpl"></bean>
					
			1-2-5) 修改Action类,依赖业务层,通过调用业务层,完成登录验证		
				
		1-3) 将Struts2和Spring框架集成在一起.
			
			拷贝一个插件jar包:struts2-spring-plugin-2.3.20.jar
				
		1-4) 将Hibernate框架集成到项目中
			
			1-4-1) 拷贝jar包
				
					antlr-2.7.7.jar					
					dom4j-1.6.1.jar
					hibernate-commons-annotations-4.0.2.Final.jar
					hibernate-core-4.2.17.Final.jar
					hibernate-jpa-2.0-api-1.0.1.Final.jar
					javassist-3.18.1-GA.jar //删除低版本:javassist-3.11.0.GA.jar
					jboss-logging-3.1.0.GA.jar
					jboss-transaction-api_1.1_spec-1.0.1.Final.jar
					
					//Spring与Hibernate集成:
					spring-jdbc-3.2.2.RELEASE.jar
					spring-orm-3.2.2.RELEASE.jar
					spring-tx-3.2.2.RELEASE.jar	
					
					//连接池:
					c3p0-0.9.1.2.jar
					
					//日志组件
					commons-logging-1.1.3.jar	
					log4j-1.2.17.jar	
					
					//驱动
					mysql-connector-java-5.0.8-bin.jar			
				
			1-4-2) 将hibernate.cfg.xml配置交给Spring统一管理,Spring框架负责管理数据源,SessionFactory对象;	
				
					<!-- 用于加载属性资源文件到Spring运行环境中 -->
					<bean class="org.springframework.beans.factory.config.PreferencesPlaceholderConfigurer">
						<property name="locations">
							<list>
								<value>classpath:jdbc.properties</value>				
							</list>
						</property>
					</bean>
					
					<!-- 声明C3P0数据源连接池组件 -->
					<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
						<property name="driverClass" value="${driverClassName}"></property>
						<property name="jdbcUrl" value="${url}"></property>
						<property name="user" value="${user}"></property>
						<property name="password" value="${password}"></property>
						<property name="maxPoolSize" value="${maxPoolSize}"></property>
						<property name="minPoolSize" value="${minPoolSize}"></property>
						<property name="initialPoolSize" value="${initialPoolSize}"></property>
						<property name="checkoutTimeout" value="${checkoutTimeout}"></property>
					</bean>
					
					<bean id="sessionFactory" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
							<property name="dataSource" ref="dataSource"></property>
							<property name="hibernateProperties">
								<props>
									<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
									<prop key="hibernate.show_sql">true</prop>
									<!-- 一个事务对应一个Session. 
									<prop key="hibernate.current_session_context_class">org.springframework.orm.hibernate4.SpringSessionContext</prop>
									-->
									<prop key="hibernate.current_session_context_class">thread</prop>
							</props>
						</property>
						<property name="mappingDirectoryLocations">
							<list>
								<value>classpath:com/bjpowernode/bean</value>
							</list>
						</property>
					</bean>
						
			1-4-3) 增加POJO类和映射配置
			
			1-4-4) 增加DAO层接口和实现类:
					将SessionFactory注入给DAO层;
			
			1-4-5) 业务层需要关联DAO,通过DAO完成数据库的登录验证.



集成原理
---------
* 


  