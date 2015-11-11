Spring + SpringMVC + MyBatis整合
===============================
1,数据源
2,sessionFactory
	<bean id="sessionFactory" class="org.mybatis.spring.SqlSessionFactoryFactoryBean>
		<property name="dataSource" ref="dataSource"></property>
		<property name="mapperLocations">
			<list>
			</list>
	</bean>
3,事务管理器
	<bean id="transactionManager" class="org.springframework.jdbc.DataSource">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
4,AOP

MyBatis
------
	<bean id="userDao" class="org.mybatis.spring.MapperSessionFactoryBean">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
		<property name="mapperInterface" value=""></property>
	<bean>

SSM:
=====
	Spring WEB 初始化容器,只会产生一个工厂对象,WebApplicationContext
	SpringMVC的DispatcherServlet读取一次配置文件
	Spring的ContextLoaderListener读取一次配置文件
	在容器对象合并的时候,注解扫描信息会丢失

	解决方案,所有的Spring的相关配置文件,使用DispatcherServlet一次性读取.