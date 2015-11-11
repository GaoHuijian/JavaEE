Tomcat源码编译
===================
1.下载源代码<Apache Tomcat官网>

2.安装配置ant

	如果机器上已经安装了ant可以省略这一步，否则去 ant官网
	(http://ant.apache.org/bindownload.cgi)下载最新版ant Binary Distributions，
	解压到某个目录下，如D:\Program Files\apache-ant-1.9.3，然后配置环境变量。
	要么新建个ANT_HOME，值为ant路径，然后在PATH中添加	ant bin路 径为%ANT_HOME%/bin
	，要么直接在PATH中添加ant bin路径为D:\Program Files\apache-ant-1.9.3\bin。
	配置好后，在cmd下运行ant  -version，如果显示版本说明配置成功，我们就可以随地使用ant了。

3.Tomcat源码解压,进入目录运行命令 : ant ide-eclipse
	
	注意:jdk版本要对应 tomcat7 ----->jdk6 jdk7
				    tomcat8 ------>jdk7


4.然后等着吧，ant会下载一堆编译Tomcat所依赖的包，下载速度根据网络情况而定。<有些包要翻墙才能下载>


5.BUILD SUCCESSFUL


6.第一种：将所需要的源码包java和test（Junit测试用例，可选）两个文件夹直接拷贝到src下。

第二种：选择File - > import -> File System，在From directory中选择tomcat源码包中的java和test两个文件夹，在Into folder中选择我们刚新建的Tomcat8项目，Finish，然后记得将这两个文件夹标记为source code（怎么标记？右键选择者文件夹 -> Build Path -> Use as Source Folder）。

当然这样过后就不是完事了，这时会发现整个项目很多红叉，也就是缺少依赖包，在Build Path里加上下面的几个依赖包即可：

Java包需要下面四个jar包，注意版本可能不一样：
ant.jar
jaxrpc.jar
org.eclipse.jdt.core_3.8.3.v20130121-145325.jar
wsdl4j-1.5.1.jar
test包里需要junit.jar，直接“Add Libraries...”选择Junit即可：

junit.jar
当然也可以在Eclipse中启动Tomcat，方法如下：

找到类：org.apache.catalina.startup.Bootstrap.java，从名字上也可以看出是启动类，如果你此时直接运行该类，会报如下错误：

Apr 02, 2014 3:27:38 PM org.apache.catalina.startup.ClassLoaderFactory validateFile
WARNING: Problem with directory [D:\workspace\Tomcat8\lib], exists: [false], isDirectory: [false], canRead: [false]
Apr 02, 2014 3:27:38 PM org.apache.catalina.startup.ClassLoaderFactory validateFile
WARNING: Problem with directory [D:\workspace\Tomcat8\lib], exists: [false], isDirectory: [false], canRead: [false]
Apr 02, 2014 3:27:40 PM org.apache.catalina.startup.Catalina load
WARNING: Can't load server.xml from D:\workspace\Tomcat8\conf\server.xml
Apr 02, 2014 3:27:40 PM org.apache.catalina.startup.Catalina load
WARNING: Can't load server.xml from D:\workspace\Tomcat8\conf\server.xml
Apr 02, 2014 3:27:40 PM org.apache.catalina.startup.Catalina start
SEVERE: Cannot start server. Server instance is not configured.
说没有配置服务器实例，从警告语句可以知道原因：当前项目路径下没有lib和conf这两个文件夹，从而找不到服务器配置文件server.xml，当然也就不能实例化服务器了。

解决方法有两个：

①将这两个文件夹直接拷贝到项目工程下

那么这两个文件夹在哪？去Tomcat源码路径里我们可以看到只有conf配置文件夹没有lib文件夹，其实这也是我编译Tomcat的原因：lib在编译后的output文件夹中的build文件夹里，conf这里也有。当然你也可以直接从官网下载二进制包，里面是编译好的Tomcat，根路径就有这两个文件夹。将build下面的conf和lib文件夹直接拷贝到项目里，再次运行，启动成功。

Apr 02, 2014 3:37:58 PM org.apache.catalina.core.AprLifecycleListener init
INFO: The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: D:\Program Files\Java\jre7\bin;C:\Windows\Sun\Java\bin;C:\Windows\system32;C:\Windows;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;D:\Program Files\TortoiseGit\bin;C:\Program Files (x86)\Windows Kits\8.1\Windows Performance Toolkit\;C:\Program Files\Microsoft SQL Server\110\Tools\Binn\;D:\Program Files\Gow\bin;D:\Program Files\Visual Leak Detector\bin\Win32;D:\Program Files\Visual Leak Detector\bin\Win64;D:\Program Files\Java\jdk1.7.0_51\\bin;D:\Program Files\apache-maven-3.2.1\bin;D:\Program Files\Git\cmd;.
Apr 02, 2014 3:38:00 PM org.apache.coyote.http11.Http11Protocol init
INFO: Initializing Coyote HTTP/1.1 on http-8080
Apr 02, 2014 3:38:00 PM org.apache.catalina.startup.Catalina load
INFO: Initialization processed in 2254 ms
Apr 02, 2014 3:38:00 PM org.apache.catalina.core.StandardService start
INFO: Starting service Catalina
Apr 02, 2014 3:38:00 PM org.apache.catalina.core.StandardEngine start
INFO: Starting Servlet Engine: Apache Tomcat/@VERSION@
Apr 02, 2014 3:38:00 PM org.apache.coyote.http11.Http11Protocol start
INFO: Starting Coyote HTTP/1.1 on http-8080
Apr 02, 2014 3:38:00 PM org.apache.jk.common.ChannelSocket init
INFO: JK: ajp13 listening on /0.0.0.0:8009
Apr 02, 2014 3:38:00 PM org.apache.jk.server.JkMain start
INFO: Jk running ID=0 time=0/29  config=null
Apr 02, 2014 3:38:00 PM org.apache.catalina.startup.Catalina start
INFO: Server startup in 386 ms
②添加VM虚拟机运行参数

第二种方法就是添加VM参数，指定这两个文件夹的具体路径，用VM的-D参数指定catalina.home属性值为具体的路径，具体方法如下：
Run as -> Run Configuration... - > Arguments -> VM arguments中设置：

-Dcatalina.home="D:/apache-tomcat-8.0.5-src/output/build"
然后Run就可以启动了。