拷贝配置文件applicationContext.xml
					applicationContext.xml
					log4j.properties
					
			1-2-3) 增加注解配置:增加context命名空间和约束文件;
			
			1-2-4) 增加包扫描配置
				
					<!-- 设置扫描父包 -->
					<context:component-scan base-package="com.bjpowernode.*" />	
												
			1-2-5) 使用常用注解 
			
					分层注解:
						@Controller		表现层
						@Service		业务层
						@Repository		DAO层
						
						@Component  //组件注解;表示声明Bean对象,不属于任何层;也可以代替分层注解;
						
					对象创建方式:
						默认:@Scope(value="singleton")
						如果希望多例:@Scope(value="prototype")	
						
					资源注解:类似于自动装配功能
						@Resource