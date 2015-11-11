Tmocat 服务器软件
======
1、服务器的配置
--------------
*  下载绿色版Tomcat压缩文件
*   解压
*   配置环境变量
*   新建变量名：CATALINA_BASE，变量值：Tomcat路径
*   新建变量名：CATALINA_HOME，变量值：Tomcat路径
*   打开PATH，添加变量值：%CATALINA_HOME%\lib;%CATALINA_HOME%\bin
*   打开命令行提示符窗口
=> 进入Tomcat安装目录==> 进入bin目录下==> 输入：service.bat install 执行即可
* 访问tomcat:
	
		http://服务器地址:8080

* ROOT目录 下,可以省略请求上下文路径.
2、Tomcat目录结构
----------
	/bin		启动、停止文件、脚本 
	/conf		配置文件 server.xml=核心配置文件.
	/lib		各种jar文件
	/logs		记录tomcat工作日志文件
	/temp		临时文件
	/webapps	应用发布目录
	/work		jsp生成的Servlet
3 web工程目录结构
----------------
		src: Java源代码
		WebRoot:工程根目录文件夹: 配置文件,页面视图文件,Java执行文件

4 Web应用的目录结构
------------------
	/					根目录、客户端都可访问
	/WEB-INF				存放各种资源，客户端不可访问，web.xml
	/WEB-INF/classes		存放class文件
	/WEB-INF/lib			存放JAR文件

4 工作原理
---------
* Sun规范Java类在web应用的工作流程,开发一套接口(Servlet)
* servlet接口,当某一个Java类被tomcat调用时,都必须提供一个service.
* tomcat管理实现servlet接口的Java类.
  + 1)用户从浏览器发送请求,请求调用某一个Java类
		http://ip地址:8080/工程名/资源路径
		资源路径:Java类别名
  + 2)tomcat接受请求,进行字符串截取操作,获得资源路径(Java类别名).
  + 3)tomcat到指定xml文件中寻找与别名对应的类名.
  + 4)tomcat根据反射机制,为当前类生成一个对象.
2)





































