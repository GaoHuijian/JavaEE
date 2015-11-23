JSP
======
JSP (Java Server Page) 缩写，是通过在HTML中嵌入Java脚本语言来响应页面动态请求。
jsp构成
=========
静态内容
--------
	HTML文本
注释
-------
	HTML注释： <!-- HTML 注释 -->
	JSP注释 ： <%-- JSP  注释 --%>
指令元素
-------
	page指令  	<%@ page language = "java" %>
	page指令  	<%@ page import = "java.util.*,java.text.*" %>
	page指令  	<%@ page contentType = "text/html;charset =GBK" %>
	include指令	<%@ include file="xxxx" %>  静态包含,一个字节码文件,导入源代码,必须注意变量名称,以及方法名称.
	taglib指令   <%/@taglib url="xxx" prifix="xxx" %>

###page指令
###include指令
###taglib指令

动作元素
-----------
	1.创建Bean:		<jsp:useBean>
	2.设置Bean属性:		<jsp:setProperty>
	3.取得Bean属性：		<jsp:getProperty>
	4.设置传送参数：		<jsp:param>

	5.页面跳转：		<jsp:forward>
	6.包含页面：		<jsp:include> 动态包含,多个字节码文件,导入结果
	7.使用appleth或javabean：<jsp:plugin>
	8.错误提示：		<jsp:fallback>
	
	9.设置标签属性:      <jsp:attribute>
	10.动态设置XML标签主体<jsp:body>
	11.动态设置XML标签   <jsp:element>
	
脚本元素
---------
	小脚本    <%   内容  ;%>
	表达式   <%=  内容  %>
	声明	  <%!  内容  ;%>
JSP内置对象
---------
	out			对象---->输出对象,
	request 	对象 :请求作用域
	response 	对象---->相应对象,文件下载,重定向
	session 	对象  :回话作用域
	application 对象  :全局作用域
	config		对象---->JSP初始化对象
	page		对象---->当前页面对象
	pagecontext	对象 :保存数据,在当前页有效.
	exception	对象---->异常信息对象