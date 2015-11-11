	1-3) 将Hibernate框架注解集成到项目中
		
			1-3-1) 拷贝jar包
					hibernate-commons-annotations-4.0.2.Final.jar
					hibernate-jpa-2.0-api-1.0.1.Final.jar
					
			1-3-2) 利用Hibernate注解代替User.hbm.xml
			
					@Entity
					@Table
					@Id
					...
			
			1-3-3) 对类进行管理,而不是映射配置管理;
					修改SessionFactory所声明Bean对象的属性注入;
					
					<!-- 设置对映射实体类进行扫描;设置包名称 --> 
					<property name="packagesToScan">
						<list>
							<value>com.bjpowernode.bean</value>
						</list>
					</property>	
					 		