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
	include指令	<%@ include file="xxxx" %>
	taglib指令   <%/@taglib url="xxx" prifix="xxx" %>
跳转指令
-----------
	1.页面跳转：			<jsp:forward>
	2.包含页面：			<jsp:include>
	3.创建Bean:			<jsp:useBean>
	4.设置Bean属性:		<jsp:setProperty>
	5.取得Bean属性：		<jsp:getProperty>
	6.使用applet插件：	<jsp:plugin>
	7.插件定义参数：		<jsp:param>
	8.插件错误提示：		<jsp:fallback>
	
脚本元素
---------
	小脚本   <%   内容  %>
	表达式   <%=  内容  %>
	声明		<%!  内容  %>
JSP内置对象
---------
	out			对象
	request 	对象
	respond 	对象
	session 	对象
	application 对象
	config		对象
	page		对象
	pagecontext	对象
	exception	对象