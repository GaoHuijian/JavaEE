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
脚本元素
---------
	小脚板
	表达式
	声明
JSP内置对象
---------
	out			对象
	request 	对象
	respond 	对象
	session 	对象
	application 对象