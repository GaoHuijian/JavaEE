1) 在Tomcat环境中使用DBCP数据源连接池组件
--------------------------------------	
		Tomcat默认集成了DBCP连接池组件的.
			%CATALINA_HOME%\lib\tomcat-dbcp.jar  
			
		DBCP(DataBase Connection Pool)	-> Apache提供连接池组件(免费)
		
		1-1) 拷贝驱动到%CATALINA_HOME%\lib\mysql-connector-java-5.0.8-bin.jar
		
		1-2) 在%CATALINA_HOME%\conf\context.xml文件中配置资源
			
			在JNDI服务中注册一个资源,注册资源名称叫作"jdbc/mysql",绑定的是一个DataSource对象.
			在Tomcat中,Tomcat实现JNDI服务接口,当Tomcat服务器启动,就将JNDI服务也启动,
			这样,我们可以在应用程序中,通过获取JNDI服务,然后通过资源绑定名称来获取这个资源;
			<Resource name="jdbc/mysql"
	            auth="Container"
	            type="javax.sql.DataSource"
	
	            username="root"
	            password="123"
	            driverClassName="com.mysql.jdbc.Driver"
	            url="jdbc:mysql://localhost:3366/test"
				initialSize="2"
	            maxActive="8"
	            maxIdle="4"
				maxWait="5000"
				/>
				
		1-3) 在应用程序中通过数据源来获取连接
		
				DBCP连接池组件实现DataSource接口.
				
				//创建初始化JNDI的上下文对象:
				Context initCtx = new InitialContext();
				//通过lookup()方法来查找具体的JNDI上下文对象
				//  Tomcat实现JNDI服务,所以,指定默认服务名称为:java:comp/env
				Context envCtx = (Context) initCtx.lookup("java:comp/env");
				
				
				//获取JDBC中的数据源对象:
				DataSource ds = (DataSource) envCtx.lookup("jdbc/mysql");
				System.out.println(ds); //org.apache.tomcat.dbcp.dbcp.BasicDataSource

				Connection conn = ds.getConnection();//从连接池中获取连接:申请连接
		
		1-4) 关闭连接
		
			conn.close(); //关闭连接:不是销毁连接对象,而是将连接归还到连接池		
		