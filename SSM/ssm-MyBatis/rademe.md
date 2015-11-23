MyBatis
========
###环境搭建步骤
--------------
* 1 导入jars包

		asm-3.3.1.jar
		cglib-2.2.2.jar
		commons-logging-1.1.1.jar
		javassist-3.17.1-GA.jar
		log4j-1.2.17.jar
		mybatis-3.2.1.jar----------------------->核心jar包
		mysql-connector-java-5.0.6-bin.jar
		slf4j-api-1.7.2.jar
		slf4j-log4j12-1.7.2.jar
* 2 配置文件
* 2.1 主配置文件 		**mybatis-config.xml**

----------------------------------------------------
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
			<properties resource="jdbc.properties" />
		<!-- 环境配置 JTA Java 全局事务 API -->
		<environments default="development">
			<environment id="development">
			<!-- 事务管理 
				field type: 事务处理方式
					JDBC :　默认情况下使用JDBC事务控制	
					MANAGER : 容器管理,如Spring
			 -->
			<transactionManager type="JDBC"/>
			<!-- 数据源
				field type : 数据源类型
					POOLED : 连接池 (C3P0  DBCP)
					UNPOOLED : 普通连接
					JNDI : java naming directory interface
						Java命名目录接口服务器
			 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}"/>
				<property name="url" value="${jdbc.url}"/>
				<property name="username" value="${jdbc.username}"/>
				<property name="password" value="${jdbc.userpswd}"/>
			</dataSource>
			</environment>
		</environments>
		<!-- 映射文件 -->
		<mappers>
			<mapper resource="mapping-beans.xml"/>
		</mappers>
	</configuration>

* 2.2 映射配置文件	**mapping-beans.xml**\
 
----------------------------------------------------
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="test">		
		<select id="getAllUsers" resultType="com.bjpowernode.beans.User">
			select * from m_user where usercode = 'admin'
		</select>		
	</mapper>
--------------------------------------------------------			
			<实体对象惯用名称>
			domain  域类型
			entity  实体类型 
			model   模型类型
			data    数据类型
	


一  :JDBC Hibernate MyBatis 传参
---------------------------------------------------------
	JDBC:   	1 ---- n    	?
	Hibernate : 0 ------n	?和:
	MyBatis:				OGNL #{}
	传递多参数:参数只与类型相关,与变量的名称和顺序无关.

#####MyBatis参数传递

---------------
*  #{}:占位符 OGNL表达式
*  如果为单个参数,可直接使用简单类型.
*  如果为多个参数,可以使用自定义类型,和Map传参.  
	* 1 使用自定义类型 要求:映射文件中的#{}的变量名称,与自定义类型中的property名称一致.
	
		User param = new User();
		param.setUsercode("xx");
		param.setUserpswd("yy");
		User resulat = session.selectOne("getUserByUsercodeAndUserpswd",param);
		
	* 2  使用Map传参 

		Map<String,Object> parm = new HashMap<String ,Object>();
		param.put("usercode","xx");
		param.put("userpswd","yy");


二  :字符串拼接传参
--------------------

####	${}

--------------------

* 传递参数不能是基本类型,只能是自定义类型,或者Map传参. 

三  :数据库相关方法|运算符传参
------------------------------
		select * from table_name like concat('%',#{},'#')	---mysql
		select * from table_name like '%'||#{}||'%'			---Oracle

四 :模糊查询
--------------
	标准传参
	拼接传参
	数据库相关方法|运算符传参
* 示例:

		MyBatis:
			1. 标准传参
				select * from table_name like #{}
				session.selectList("", "%" + paramValue + "%");
			2. 拼接传参
				select * from table_name like '%${}%'
				session.selectList("", Map[DefinitionObject]);
			3. 数据库相关方法|运算符传参
				select * from table_name like concat('%', #{}, '%')  -- MySQL
				select * from table_name like '%' || #{} || '%'  -- Oracle
				session.selectList("", paramValue);

五 :结果类型
------------
* 自定义类型 :进行查询数据赋值的时候,是根据字段的名称,能否使用getter/setter方法中返回类型中访问,是否进行数据的赋值.
* Map类型
* 字符串  : 返回查询结果的第一列
* 基本类型

六 :结果自定义类型映射配置
-----------------------------
		<resultMap type="xx"  id="">			
			 <id column="" prototype="">
		     <result column="" prototype="">
		</resultType>
七 :分页查询
-------------
	MYSQL:
			select column from table
					where  条件
					group by 分组
					having 组条件
					order by 排序
					limit startIndex, rowCounts
	Oracle:
		select colum  from
			(select column from
					(select column ,rownum as rn from table) t1
				where t1.rn < ?)
			
		where t1.rn > ?

-------------------------------------------------

*  物理分页:依赖数据的分页语法进行部分数据查询的分页,
	*  效率相对较低,
	*  内存压力小,
	*  移植性差
 
*  内存分页:是MyBatis会将需要查询的指定数据之前的所有信息查询,并多查询一条数据.
	* 在内存中,使用循环遍历的方式,将指定的分页数据进行封装,并返回.
	* 相对效率高,但是对内存压力大.

八 :动态查询 <查询条件数量不确定>
--------------------------------
	• if
	• choose (when, otherwise)
	• trim (where, set)
	• foreach
九 :CUD底层都是调用JDBC的 executeUpdate();

* C:create
* U:update
* D:drop
* 示例:
		<delete id="deleteUser">
			delete from m_user
				where usercode = #{usercode}
		</delete>
		
		<update id="updateUser">
			update m_user
				set userpswd = #{userpswd}
				where usercode = #{usercode}
		</update>
		
		<insert id="insertUser">
			insert into m_user(usercode, userpswd, username)
				values( #{usercode}, #{userpswd}, #{username} )
		</insert>



十 :缓存
---------
* cache
	* 一级缓存 sqlSession缓存,默认开启,支撑查询语句,缓存标志是标签的ID属性值
	* 二级缓存 SqlSessionFactory缓存,默认关闭,需配置开启,可以跨越多个session使用
		* 在映射配置文件中增加标签<cache/>


	* 缓存策略:eviction--->缓存移除策略
		* LRU :将缓存中使用次数少的删除,使用次数频繁的保留,默认策略
		* FIFO:队列模式,先进先出
	* flushInterval:刷新周期,单位毫秒
	* size:缓存数据空间大小,默认值是1024,单位对象个数	

十一:事务
--------	
* 默认事务-->默认回滚
	* 在回话关闭时,session.close()执行的时候,会检查事务

* 手动事务
	* 成功提交 session.commit();
	* 异常自动回滚 /最好: session.rollback();

十二:关联关系
------------
* one to one 
* many to one
* one to many
* many to many
* self relation

------------------

####MyBatis
* many to one
	* 不会主动的封装加载关系对象.
* one to many
* one to one




十三:延迟加载
-------------
* 默认关闭
* 配置打开

	<settings>
		<setting name="lazyLoadingEnabled" value="true"/>	//开启延迟加载,默认值false
		<setting name="aggressiveLazyLoading" value="false"/> //侵入性延迟加载,默认为true 一旦访问主对象的属性,是否加载关系对象数据
	</settings>

十四:
-------------


十五:主键同步
---------------
* Hibernate :三状态
* MyBatis: 默认不会进行ID数据的同步
		 <selectKey keyProperty="id" resultType="属性类型">
		 	MySQL:
				select @@identity as id
			Oracle:
				select seq_name.currVal from dual
		 </selectKey>