XML文件
======
* 用来保存数据之间隶属关系
* XML使用标签形式保存数据
* XML标签,不仅可以保存数据,还可以保存子标签.
* XML标签与标签之间关系:
	* 兄弟关系
	* 父子关系
* XML文件,都应该有一个根目录标签

读取XML文件
==========
###SAX.jar
###dom4j.jar

* SAXReader    :XML文件读取
* org.dom4j.Document:表示XML文件
* org.dom4j.Element:一对标签就代表一个标签对象
* org.dom4j.Attribute:表示标签中属性类型.


步骤:
====
1)创建一个XML文件的读取器对象.
2)创建一个I/O流,指向硬盘上XML文件.
3)读取器对象,通过I/O流将硬盘上XML文件,读取内存.
4)读取XML根目录标签.
5) 读取根目录标签下的子标签.


XPATH
=====
* 处理xml 节点的一个 jar包.

xml属性/标签
-----------
		 1.创建xml文件读取器对象
		 2.创建I/O流,指向硬盘上xml文件
		 3.读取器对象,通过I/O流将硬盘上xml文件读取到内存中.
		 4.获得xml文件根目录标签.
		 5.读取当前标签下,指定名称的子标签
		 6 读取标签中属性
用到的类:SAXReader Document Element Iterator Attribute












