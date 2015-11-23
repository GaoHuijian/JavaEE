Servlet各个对象:

ServletContext		application  所有用户共享对象
HttpSession 		session	 单个用户
HttpServletRequest  	request 	 单个请求
PageContext			page		 当前页面
Cookie			cooki		 用户浏览器
ServletConfig				 单个Servlet
ServletResponse				 单个响应

JSP内置对象:

		out			JspWriter
		response		HttpServletResponse
		config		ServletConfig
		exception		Throwable           默认没有此对象 is ErrorPage="false"
		page			this (HttpJspBase) 很少用

		pageContext		PageContext
		request		HttpServletRequest
		session		HttpSession
		application		ServletContext

EL表达式对象:


EL							JSP
-----------------------------------------------------
applicationScope					application
sessionScope					session
requestScope					request
pageScope						pageContext


* EL只能通知tomcat读取四大作用域中的数据,但不能修改四大作用域中的内容
* ${EL作用域名称,关键字}
  ${关键字}  :tomcat 1) pageContext 2) request 3) session 4) application



EL 隐藏对象
pageContext  :读取JSP中pageContext中基本信息
param		 : 通知tomcat调用request.getParmeter("参数信息")读取请求参数值
