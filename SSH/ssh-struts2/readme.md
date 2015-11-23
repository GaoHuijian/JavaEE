Struts2主要作用
===============
	Action配置
	结果配置

* ActionInvocation作用:保存当前执行环境
	 
配置核心控制器
----------
	<filter>
	<filter-name>struts2</filter-name>
	<filter-class>
	org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
	<filter-class>
	</filter>

	<filter-mapping>
	<filter-name>struts2</filter-name>
	<url-pattern>/*</url-pattern>
	</filter-mapping>	

配置Action
----------
	<package name="example" namespace="/example" extends="struts-default">

	<action name="HelloWorld" class="com.bjpowernode.web.action.HelloWorldAction">
	<result>/WEB-INF/jsp/example/HelloWorld.jsp</result>
	</action>

	</package>


Struts常量配置
------------
*  如何修改扩展名称 :<constant name="struts.action.extension" value="do"></constant>

*  重加載配置:<constant name="struts.configuration.xml.reload" value="true"></constant>

*  开发模式:<constant name="struts.devMode" value="true"></constant>

*  命名空间namespace

*  包package

Struts2 数据流转
----------------
* 框架通过值栈和Ognl实现数据流转过程的. 

	1-1) 值栈:ValueStack  (实现类:OgnlValueStack) 操作数据特点:先进后出
		
		栈顶:索引为0位置

			push(Object o)   
			将对象存放在值栈中,也称为压栈.
			将每一个元素存放在栈内存空间中时,每次将元素存放在索引为0位置,在存放时,其他元素要向栈底移动一个位置.

			Object o = pop()
			将栈顶元素取出返回,并且从栈顶删除这个元素.也称为弹栈.
			每次获取栈内存空间数据时,只能获取索引为0位置元素,获取后将其从栈内删除,其他元素向栈顶移动一个位置.

			Object o = peek() 
			将栈顶元素取出返回,栈里元素不变.(只能取索引为0位置(栈顶)的数据.)
			每次获取栈内存空间数据时,只能获取索引为0位置元素,栈内所有元素不变.
				
			框架默认将每次创建的Action对象存放在值栈的栈顶.	

* 值栈对象的创建:

			Ognl可以操作值栈中的数据:
				值栈分成两个部分:

					ValueStack : 狭义值栈.
					ActionContext : 上下文集合(包含ValueStack).  广义值栈.

			
			框架利用Ognl来操作与这次请求有关所有数据.一般情况下,
				public class ActionContext{
					private Map<String, Object> context; //放了值栈对象,存放了request,session,application这些范围的数据.
					public Map<String, Object> Map() {
				        return context;
				    }
			    }
			
				ActionContext ac = ActionContext.getContext();
				Map context = ac.getContextMap(); //ognl.OgnlContext implements Map
				//框架的ActionContext集合是通过OnglContext上下文集合实现的.
				所以,可以通过Ognl来操作ActionContext上下文集合.
				
*  数据流转(跟值栈和Ognl有关)
	
		2-1) 数据提交
		
			客户端如何将数据提交给服务端 : B/S
				2-1-1) <form>表单:
							  action="<%=request.getContextPath()%>/user/login.action"
						<form action="http://localhost:8080/ContextPath/user/login.action" method="post">
				   			用户代码:<input type="text" name="usercode" value="zhangsan"><br>
				   			用户密码:<input type="password" name="userpswd" value="123"><br>
				   				<input type="submit" value="登录">
				   		</form>
				   		
				2-1-2) 地址栏:http://localhost:8080/ContextPath/user/login.action?usercode=zhangsan&userpwd=123 		
						
				2-1-3) 连接<a href="<%=request.getContextPath()%>/user/login.action?usercode=zhangsan&userpwd=123">登录</a>		
			
				2-1-4) 异步js:
					   XMLHttp.open("post", "<%=request.getContextPath()%>/user/login.action?usercode=zhangsan&userpwd=123", true);					   
					   XMLHttp.send(); 
		
		2-2) 数据存储(数据如何接收到的)
			最基本原理:通过request对象来获取请求参数.
					 public String getParameter(String name); //一次只能获取一个参数的值.
					 public String[] getParameterValues(String name); //一次获取一个参数名称的所有的值
					 public Enumeration<String> getParameterNames(); //一次性获取了所有参数名称.
					 public Map<String, String[]> getParameterMap(); //一次性获取了所有参数名称及参数值
					 
			框架对请求参数的获取进行封装.
				提供一个 参数拦截器,	拦截请求,从请求对象中获取所有请求参数,存放在集合中,然后对集合进行迭代,
				获取每一个参数及参数值,然后调用Ognl将每一个参数封装到值栈栈顶的Action对象中.	
				for(){//对参数集合进行迭代
					Ognl.setValue("usercode",action,"admin"); //Ognl.setValue("userpswd",action,"admin");
				} 
				
			2-2-1) 属性驱动模式(参数比较少时比较适合)
			
					使用方式:
							<input type="text" name="usercode" value="admin">
							
					Action开发:
							public class LoginAction{
								private String usercode;
								public void setUsercode(String usercode){
									this.usercode=usercode ; //成员变量接收表单参数的值
								}
							}		
			
			2-2-2) 模型驱动模式(参数比较多的时候比较适合使用,具有一定侵入性,不推荐使用)
			
					使用方式:
							<input type="text" name="usercode" value="admin">
							<input type="text" name="userpswd" value="admin">
							
					Action开发:
							public class LoginAction2 implements ModelDriven<User> {
								private User user ; //  user对象的get/set方法不是必须的.
								public User getModel(){ //模型驱动拦截器调用该方法,将user获取后存放栈顶.
									user = new User();
									return user ; //而且不能为null
								}
							}	
							
							public class User{ //将表单多个参数的属性及set方法定义到一个模型对象中.
								private String usercode;
								private String userpswd;
								set方法必须的,get方法是可选择的.						
							}
							
					原理:
						框架提供一个叫做  模型驱动拦截器(ModelDrivenInterceptor L94),拦截请求,判断Action对象是否属于ModelDriven类型的(action instanceof ModelDriven).
						调用模型调用接口的getModel()方法,获取数据模型对象(user对象),将它压入值栈,栈顶就是user对象;
						
						然后,再通过一个 参数拦截器(ParametersInterceptor L221),	拦截请求,从请求对象中获取所有请求参数,存放在集合中,然后对集合进行迭代,
							获取每一个参数及参数值,然后调用Ognl将每一个参数封装到值栈栈顶的user对象中.	
							for(){//对参数集合进行迭代
								Ognl.setValue("usercode",user,"admin"); //Ognl.setValue("userpswd",user,"admin");
							} 

				2-2-3) 域驱动模式(参数比较多时使用,无侵入性;推荐使用)重点
					
					使用方式:
							<input type="text" name="user.usercode" value="admin">
							<input type="text" name="user.userpswd" value="admin">
							
					Action开发:
							public class LoginAction3{
								private User user ; //  user对象的get/set方法是必须的.
								get/set方法
							}	
							
							public class User{ //将表单多个参数的属性及set方法定义到一个模型对象中.
								private String usercode;
								private String userpswd;
								set方法必须的,get方法是可选择的.						
							}
				
					原理:
						通过一个 参数拦截器(ParametersInterceptor L221),	拦截请求,从请求对象中获取所有请求参数,存放在集合中,然后对集合进行迭代,
							获取每一个参数及参数值,然后调用Ognl将每一个参数封装到值栈栈顶的Action对象中.	
							for(){//对参数集合进行迭代
								Ognl.setValue("user.usercode",action,"admin"); //Ognl.setValue("user.userpswd",action,"admin");
							} 
						
							
		2-3) 数据传播
		
			最基本原理:
				request.setAttribute("key",value);   //Object value = request.getAttibute("key");   // ${requestScope.key}
				session.setAttribute("key",value);   //Object value = session.getAttibute("key"); 	//${sessionScope.key}
				application.setAttribute("key",value); //Object value = application.getAttibute("key"); //${applicationScope.key}
		
			框架中进行数据传递存在两种方式:
				
				2-3-1) 第一种:依然采用HTTP对象(特殊场合下使用:例如获取客户端IP地址,获取uri,设置请求字符编码,获取输入输出流)
				
						//获取HTTP对象又存在三种方式:
						//1.Action实现特定接口,来获取Http对象;例如:ServletRequestAware,ServletResponseAware接口;
							//这种方式与一个拦截器有关:org.apache.struts2.interceptor.ServletConfigInterceptor
							//HttpServletRequest request = this.request ;
							this.request.setAttribute("username", "zhangsan"); //request范围: ${requestScope.username}
							
							HttpSession session = this.request.getSession(true); //session范围  ${sessionScope.username}
							session.setAttribute("username", "lisi");
							
							ServletContext application = session.getServletContext(); //application范围 : ${applicationContext.username}
							application.setAttribute("username", "wangwu");
						
							//response.getWriter();
							//response.getOutputStream();
					
						//2.通过ActionContext来获取
						
							/*ActionContext ac = ActionContext.getContext();
							HttpServletRequest request = (HttpServletRequest)ac.get(StrutsStatics.HTTP_REQUEST);
							HttpServletResponse response = (HttpServletResponse)ac.get(StrutsStatics.HTTP_RESPONSE);*/
					
						//3.通过一个工具类来获取:ServletActionContext(推荐)
							//HttpServletRequest request = ServletActionContext.getRequest();
							//HttpServletResponse response = ServletActionContext.getResponse();	
				
				
				2-3-2) 第二种:采用集合(框架对HTTP对象的使用做了封装)	 推荐使用	
					
						ActionContext ac = ActionContext.getContext();
						ac.put("username", "zhangsan"); //相当于将数据存放在request范围   :  ${requestScope.username}
						
						Map sessionMap = ac.getSession();
						sessionMap.put("username", "lisi"); //session.setAttribute(key, value);相当于将数据存放在session范围  ${sessionScope.username}
						
						Map applicationMap = ac.getApplication();
						applicationMap.put("username", "wangwu"); //相当于将数据存放在application范围 : ${applicationContext.username}
		
						原理: 
							框架会将request,session,application范围的数据封装成集合的形式.最终存在在一个大的Map集合中.
								Dispatcher L624
		
		2-4) 数据展示			
		
			获取Action对象属性值,或所关联对象属性值: 从ValueStack中获取数据.
				如果是采用属性驱动模式(栈顶Action对象): ${usercode}<br>
		   		如果是采用模型驱动模式(栈顶User对象): ${usercode}<br>
		   		如果是采用域驱动模式(栈顶Action对象): ${user.usercode}<br>

			获取HTTP对象范围的数据:
		   		从小到大的范围搜索数据:${username}<br>
		   		从request范围获取数据:${requestScope.username }<br>
		   		从session范围获取数据:${sessionScope.username }<br>
		   		从application范围获取数据:${applicationScope.username }<br>
			
			
			例如:
				如果利用${usercode}获取数据:原理是什么?	
					
					${usercode} => 
						<%= request.getAttribute("usercode") %>;
			
						当从request范围获取数据时,实质上调用是框架所包装的请求对象(StrutsRequestWrapper)的重写的getAttribute()方法.
						而getAttribute()方法从值栈中获取数据 -> 
								String usercode =  (String)Ognl.getValue("usercode",action);
			
			
					${user.usercode} => 
						<%= request.getAttribute("user.usercode") %>;
			
						当从request范围获取数据时,实质上调用是框架所包装的请求对象(StrutsRequestWrapper)的重写的getAttribute()方法.
						而getAttribute()方法从值栈中获取数据 -> 
								String usercode =  (String)Ognl.getValue("user.usercode",action);
				



结果类型
-------
	框架封装11个结果类型,参考:struts-default.xml		
		<result-types>
            <result-type name="chain" class="com.opensymphony.xwork2.ActionChainResult"/>
            <result-type name="dispatcher" class="org.apache.struts2.dispatcher.ServletDispatcherResult" default="true"/>
            <result-type name="freemarker" class="org.apache.struts2.views.freemarker.FreemarkerResult"/>
            <result-type name="httpheader" class="org.apache.struts2.dispatcher.HttpHeaderResult"/>
            <result-type name="redirect" class="org.apache.struts2.dispatcher.ServletRedirectResult"/>
            <result-type name="redirectAction" class="org.apache.struts2.dispatcher.ServletActionRedirectResult"/>
            <result-type name="stream" class="org.apache.struts2.dispatcher.StreamResult"/>
            <result-type name="velocity" class="org.apache.struts2.dispatcher.VelocityResult"/>
            <result-type name="xslt" class="org.apache.struts2.views.xslt.XSLTResult"/>
            <result-type name="plainText" class="org.apache.struts2.dispatcher.PlainTextResult" />
            <result-type name="postback" class="org.apache.struts2.dispatcher.PostbackResult" />
        </result-types>

 2) 常用结果类型
    
    	2-1) dispatcher
    		默认类型.
    		用于转发视图(JSP).			request.getRequestDispatcher("/success.jsp").forWord(request,response);
    		默认不能转发Action.
    		过滤器对转发默认是不进行过滤的.那么,也就不会解析uri,当然也就查找不到action.
    	
    			如果希望可以转发Action,那么,需要修改过滤器过滤规则;之后,直接添加action地址  /xxx.action  | /xxx/yyy.action 即可转发.
    				<dispatcher>REQUEST</dispatcher><!-- 对请求进行过滤,默认值 -->
    				<dispatcher>FORWARD</dispatcher><!-- 服务器内部资源转发,也要执行过滤器 -->
    				
    				/* 			对所有请求过滤,扩展匹配特殊方式.
    				/test/*		扩展匹配,多/test路径的所有资源过滤
    				*.action	后缀匹配,匹配所有以.action结尾请求
    				/test/xxx	精确匹配,匹配一个具体路径
    	
    	2-2) chain
    	* 会将数据传递
    		<!-- type="chain" 主要用于转发Action. 
				如果是同一个命名空间的Action,那么,只需要指定action的名称.			
			<result name="success" type="chain">query</result> 
			-->	
			<!-- 如果两个Action不在同一个命名空间下,需要设置两个参数 -->
			<result name="success" type="chain">
				<param name="actionName">query</param>
             	<param name="namespace">/test</param>
			</result> 
    	
    	2-3) redirect
    	
    		response.sendRedirect("http://www.baidu.com");
    		response.sendRedirect("/success.jsp"); //从ROOT下查找资源
    		response.sendRedirect(request.getContextPath()+"/success.jsp"); //从ROOT下查找资源
    		
    		<!-- type="redirect" 可以重定向任何路径资源;
				如果两个action在同一个命名空间下,直接指定action名称就可以了,
				如果两个action不在同一个命名空间下,只需要设置完整路径就可以了.
				这种设置请求路径必须手动增加合法扩展名称,一旦扩展名称发生变化,那么,路径也需要改变,属于耦合性开放.				
			<result name="success" type="redirect">/test/query.action</result> 
			 -->
			 
			<result name="login" type="redirect">/login.jsp</result>
    	
    	2-4) redirectAction
    	
    		主要用于重定向Action.
    		
    		<!-- 
			 	type="redirectAction"
			 		如果两个action在同一个命名空间下,直接指定action名称就可以了
			 		如果你两个Action不在同一个命名空间下,需要设置两个参数
			 		<result name="success" type="redirectAction">query</result>  
			  -->
			<result name="success" type="redirectAction">
				<param name="actionName">query</param>
             	<param name="namespace">/test</param>
			</result>  	

		2-5)stream		 

拦截器
-----
	1) Struts2框架的拦截器
	
		框架默认定义了35个拦截器.框架设置了默认拦截器引用.默认引用20个拦截器.
		
	2) 自定义开发拦截器
	
		2-1) 开发拦截器
			
			需要实现拦截器接口:	com.opensymphony.xwork2.interceptor.Interceptor
			也可以继承拦截器抽象父类:com.opensymphony.xwork2.interceptor.AbstractInterceptor
			
			推荐继承父类AbstractInterceptor.可以不用对init(),destroy()方法进行实现了.体现了默认适配器模式.
			
		2-2) 声明拦截器
			
			<!-- 声明拦截器或拦截器栈 -->
			<interceptors>
				<interceptor name="logInterceptor" class="com.bjpowernode.web.interceptor.LogInterceptor"></interceptor>
			</interceptors>	
			
		2-3) 在Action配置中引用拦截器
		
			<!-- 引用拦截器:引用到拦截器就会对目标对象中方法进行拦截. -->
			<interceptor-ref name="logInterceptor"></interceptor-ref>	
			
			由于配置自己拦截器引用,那么框架默认拦截器引用就失效了.	
			
	3) 如果希望框架默认拦截器还继续起作用?
		
		3-1) 可以将框架默认20个拦截器一一引用一遍 
		3-2) 直接引用默认拦截器栈
		3-3) 可以引用自己定义的拦截器栈
		3-4) 设置自己默认拦截器引用
			 <default-interceptor-ref name="myStack"></default-interceptor-ref>		
			
	4) 拦截器开发与应用需要注意哪些问题?
		
		4-1) 拦截器对象是在服务器启动时创建的.单例的.对象创建后马上执行init()方法(只执行一次).
		4-2) 拦截器执行顺序与配置拦截器引用顺序有关.
			多个拦截器执行,形成一个拦截器链;这里体现了一个叫做责任链设计模式.
			
				过滤器,拦截器都体现责任链设计模式.
				
				责任链设计模式 : 
					将复杂代码拆分成多个相同元素,将这些元素纳入一个链式结构中来顺序执行.
					每一个元素都责任调用下一个元素的执行.
					这种模式好处,可以将复杂问题简单化,可以任意调整元素顺序,可以任意增加或减少元素个数,具备高度扩展性.
					使代码复用性提高,提高开发效率.
					
		4-3) 拦截器只对Action请求进行拦截,对jsp请求是不拦截的.	
				与Filter不同,过滤器可以对任意请求资源进行过滤.
				
		4-4) 一旦配置自己拦截器引用,那么框架默认拦截器引用就失效了.	
		
		4-5) 每一个拦截器拦截方法必须执行invocation.invoke();否则,后续拦截器得不到执行.		
		

* Struts2用到的设计模式

	责任链
	缺省适配器
	代理
	工厂
	模板
	ThreadLocal

* Struts2配置文件使用${name} 表示 调用getName()方法
* Spring配置文件使用$(name) 表示 property里的参数

* 配置文件 struts-default.xml       default.properties

* 优化  减少拦截器


* Struts2多例	
	* action 	是多例	有状态类 	<拥有可以修改的成员变量>
	* Servlet 	是单例 	是无状态类	

* 优点
	* 开发快




























