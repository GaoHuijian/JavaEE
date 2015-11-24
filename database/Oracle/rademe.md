数据类型
--------
* number
* char
* varchar
* varchar2
* date 日期类型:实际开发的时候,日期数据一般用char类型.
* Blob  ----->Binary Large Object(二进制大数据对象) 一般用来存储声音\图片\视频等二进制数据
* Clob  ----->Character Larget Object(字符大数据对象) 主要用来存储大文本




conn 
show user


sqlplus命令:
set linesize 200
set pagesize 50

	list 	 : 查看缓存里的sql命令
	l		 : 查看缓存里的sql命令

	run 	 : 执行缓存里的sql命令
	r  		 : 执行缓存里的sql命令
	/ 	  	 : 执行缓存里的sql命令

	save 文件: 保存命令
	get 文件 : 从文件执行sql命令
	edit    : 修改缓存sql命令
	ed      : 修改缓存sql命令
	@文件    : 执行文件里的sql命令

	clear Screen:清屏
	cle scr		:清屏

Oracle用" "地方:别名

	运算符:

	null表示无穷大 / 任意值

	区分大小写


	% 任意多个字符
	_ 一个字符.


	转义:escape '@' '#'

	desc 降序
	esc 升序

	伪表: dual;

	nvl

	分支:case when then else end

	decode


to_data

to_char 
* 大多使用查询操作中,如果默认的显式格式不符合用户要求,可以手动调用to_char方法进行转换

to_number


* distinct 去重
	

* 原则:可以在where过滤的数据就不要用having过滤



连接查询的分类
---------------
* SQL92
* SQL99

--------------------------------------

* 内连接:等值连接非、等值连接、自连接
* 外连接:做外连接、右外连接
* 任何一个左外连接都会对应一个右外连接

rownum 行号<隐含字段> 在数据库的每张表中都存在
* 完成分页功能
* 必须从1开始
*  =1 <= <  >=1




分页查询:

pageNO       startline                 endLIne               pageSize
  
  x		开:(x-1)* pageSize          闭: x * pageSize


rowId:

* 是Oracle数据库特有的内容,表示表中的该条记录在硬盘上存储的真实物理地址
* 通过物理地址定位表中的记录,效率较高