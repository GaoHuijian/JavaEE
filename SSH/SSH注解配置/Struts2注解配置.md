	1) Struts2框架注解引用开发
	
		利用注解代替XML配置文件,简化配置,提高开发效率.
		
		1-1) 引入注解jar包:struts2-convention-plugin-2.3.20.jar
		
		1-2) 遵循注解应用开发原则:
			
			1-2-1) Action所在的包的名称应该包含特定名称
			
				<constant name="struts.convention.package.locators" value="action,actions,struts,struts2"/>	
				
			1-2-2) Action类需要实现Action接口,或者类名称以Action结尾.	
			
		1-3) 利用框架注解代替xml完成相关配置
			
			@ParentPackage(value="struts-default")	//表示继承父包.
			@Namespace(value="/user") //表示声明命名空间
			
			@Action( //表示Action配置
					value="login",	//声明Action名称
					results={
							@Result(name="success",type="dispatcher",location="/userManage/success.jsp"), //表示声明结果配置
							@Result(name="login",type="redirect",location="/login.jsp")
					}
			)	
			
	2) 注解和XML比较?
	
		注解:
			优点:
				是一种引用数据类型,符合面向对象编程特点.
				利用反射机制解析类,读取注解配置.效率高.	
				可以提高开发效率.
				提早发现错误.解决问题.

			缺点:
				配置散落在代码中,配置不集中.
				如果没有源代码,看不到配置.
				配置变化需要修改类,违背OCP原则.
				
		XML: 
			优点:
				功能强大.可以用于作配置文件,可以作为规范.是一种通用数据格式.
				扩展性强.
				利用DOM/SAX解析.
				配置比较集中,一目了然.
				
			缺点:
				解析速度慢,比较复杂,解析不容易.
				配置出错不容易发现.
				面向结构化的编程方式.
		
		如何选择?
			不变的配置采用注解,否则用XML.			
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
			
			
			