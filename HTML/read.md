客户端浏览器向服务端发送数据方式:
===============
1)Form表单形式,发送数据
---------------------

2)通过浏览器地址栏,添加参数
------------------------
URL: http://localhost:8080/工程名/资源路径?请求参数=数据&请求参数=数据

3)通过超链接
----------
<a herf="工程名/资源路径?请求参数=数据&请求参数=数据">链接</a>


异常
===
404
500


中文转码
=======
Servlet默认:ISO-8859-1 
浏览器:
数据库:
Get/Post 转码:

	byte byteArrar[] = str.getBytes("iso-8859-1");
	newStr = new String(byteArray,"utf8-8");
* 缺点: 如果转码的字段较多,涉及的代码比较巨大.

* GET方式,进行转码:

 		tomcat/conf/servers.xml  添加:URIEncoding="utf8";

* POST方式:在servletquest读取数据之前,通知请求对象使用字符集(utf-8/GBK)
		
	req.setCharacterEncode("utf-8"); 必须在读取数据之前






