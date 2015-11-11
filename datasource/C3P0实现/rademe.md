	1) 在Spring框架中管理数据源对象
	
		1-1) DBCP连接池组件使用
	
			1-1-1) 拷贝jar包
			
					commons-dbcp-1.4.jar
					commons-pool-1.6.jar
					
			1-1-2) 声明数据源对象
				
				<!-- 用于加载属性资源文件到Spring运行环境中 -->
				<bean class="org.springframework.beans.factory.config.PreferencesPlaceholderConfigurer">
					<property name="locations">
						<list>
							<value>classpath:jdbc.properties</value>				
						</list>
					</property>
				</bean>
				
				<!-- DBCP数据源连接池组件 -->
				<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
					<property name="driverClassName" value="${driverClassName}"></property>
					<property name="url" value="${url}"></property>
					<property name="username" value="${user}"></property>
					<property name="password" value="${password}"></property>
					<property name="maxActive" value="${maxActive}"></property>
					<property name="maxIdle" value="${maxIdle}"></property>
					<property name="initialSize" value="${initialSize}"></property>
					<property name="maxWait" value="${maxWait}"></property>
				</bean>		
		
			1-1-3) 通过数据源获取连接
				
				ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		
				DataSource ds = (DataSource)ac.getBean("dataSource");
				
				Connection con = ds.getConnection();//从连接池中获取连接
				
			1-1-4) 关闭连接
			
				con.close();//归还连接	
		
		1-2) C3P0连接池组件使用
			
			1-2-1) 拷贝jar包:c3p0-0.9.1.2.jar
			
			1-2-2) 声明C3P0数据源Bean对象
				
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
			