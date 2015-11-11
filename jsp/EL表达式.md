EL表达式
* ${表达式}
* 表达式:算术运算,关系运算,逻辑运算表达式,三元运算表达式

四大对象代替四大作用域

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
