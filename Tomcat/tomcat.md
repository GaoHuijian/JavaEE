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
2、Tomcat目录结构
----------
	/bin		启动、停止文件、脚本 
	/conf		配置文件
	/lib		各种jar文件
	/logs		日志文件
	/temp		临时文件
	/webapps	应用发布目录
	/work		jsp生成的Servlet

3 Web应用的目录结构
------------------
	/					根目录、客户端都可访问
	/WEB-INF				存放各种资源，客户端不可访问，web.xml
	/WEB-INF/classes		存放class文件
	/WEB-INF/lib			存放JAR文件

4 
---------------------
