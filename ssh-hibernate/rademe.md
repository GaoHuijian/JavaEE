Hibernate 框架
============

1 部署
-----
* 开发简单:不许要写sql 移植性好 对象化
* flush() 作用:生成sql,执行sql <默认顺序? insert update delete>
* hibernate实体可以采用final修饰吗?       可以 ,但是延迟加载就会失效
* get()和load()方法区别:
			* 都是主键查询
			* get查不到返回null							不支持延迟加载
			* load查不到,返回代理对象,使用时会抛异常		支持延迟加载
* OpenSessionInView的工作原理
	缺点:将事务延迟到view, 占用资源  解决:将Filter配置到需要延迟加载的地方.

* 悲观锁

* 乐观锁		版本号,时间戳

* 映射
	* 一对多  ---------------------> 让多的一方维护
	* 多对多
	* 多对一
* query.list() query.iterate()
	* 区别 list()   带缓存,但默认不利用缓存 
	* iterate()	  		    缓存,利用缓存
* N+1问题 如何优化,如果有缓存则iterate 没有则list 
	* 先查主键, 然后在缓存中查询,如果没有再去数据库查询.
* 缓存:
	* 一级缓存 session          	缓存:实体对象				|
	* 二级缓存 sessionFactory	缓存:实体对象				| 默认开启,不太适合生成环境
	* 查询缓存 					缓存:普通属性,或投影缓存	|

* 缓存如何实现:
	* 将数据放在集合中<Map>
	* 重要数据尽量不用缓存.

* 优化hibernate
	* 缓存
	* N+1
	* 延迟加载

* SessionFactor
* session

* inverse 影响	存贮
* cascade 		增删改
* lazy     		查询




2 配置
-------
* 核心配置hibernate.cfg.xml
* 定义POJO类:(与表进行映射的类)
* 定义映射配置文件
* 在主配置文件中增加对映射配置文件的管理


3 主键配置
-----------
* 3-1) 数据库维护主键   (代理主键)

	native : 表示根据方言来决定采用哪种主键生成策略:	重点
					
	如果采用MySQL方言,那么就会自动选择identity
	如果采用Oracle方言,那么就会自动选择sequence
				
	identity : 表示由数据库的自动增长来分配主键.auto_increment	 适合用于MySQL,SQL Server	
					
	主键值可能不连续,如果自己修改过主键,有可能造成主键冲突.			
				
	sequence : 表示采用数据库序列来分配主键.适合用于Oracle,DB2		
				
	数据库维护主键,要求主键字段类型是整型	
			
* 3-2) 框架维护主键(一般代理主键)
		
	increment : 获取表主键字段值的最大值,然后+1分配主键
	select max(id) from t_user
							
	多事务并发执行insert时,主键分配有可能冲突	
							
	主键字段类型是整型		
		
	uuid : 生成速度快,表示由框架根据参数(IP,JVM启动时间,系统时间,计数器)动态生成128bit,32长度16机制的数值字符串.
	1111	->	15
	0000	-> 	0 
					
	要求主键字段类型:字符串,数据量大.
	插入快,查询慢
		
* 3-3) 自己维护主键(一般自然主键)
			
	assigned : 业务维护主键

* 4 POJO对象三个状态
--------------
		4-1) 瞬时状态(临时状态)
		
			一定没有被框架管理
			数据库里没有记录
		
		4-2) 持久化状态
		
			一定被框架管理
			数据库里一定有记录存在
		
		4-3) 游离状态(脱管状态,离线状态)
			
			一定不被框架管理(session.close()/session.clear())
			数据库里一定有记录存在


5 数据库查询
-----------	
		1-1) get()	主键查询
		
			返回结果:
				POJO对象或null(非空判断)
		
		1-2) load()
		
			支持延迟加载功能.
			
			返回结果:
				代理对象或异常或POJO对象(禁用)
				
	2) HQL 
	
		Hiberante Query Language   是一个面向对象的查询语言.Hibernate框架提供的.
		对SQL进行封装,语法类似于SQL语句,最终HQL需要翻译课程SQL才能进行数据库数据操作.	
		
			SQL : select u.username,u.age from t_user u where u.id=1
			HQL : from User u where u.id=1
					select u.name,u.age from User u where u.id=1
		

5 表关系映射
-----------
* 多对一

	1) 框架关联关系映射
		
		在内存中产生多个对象时,对象之间存在哪些关联关系?
			多对一,一对多,一对一,多对多
			
		面向对象模型中描述对象之间的关系存在单向和双向两种;	
		
	2) 多对一(学生和班级)单向关联
		
		2-1) 从对象模型角度	
		
			在多的一端增加对一的一端的对象引用.
				
		2-2) 从关系模型角度
			
			在学生表中增加对班级表一个引用的外键.	
				
		2-3) 从映射模型角度	
		
			<!-- 多对一关系映射:外键映射 
			外键字段值,默认来自关联班级对象主键属性值;
			-->
			<many-to-one name="classes" column="cid" class="com.bjpowernode.hibernate.pojo.Classes"></many-to-one>	
* 一对多
 
	1) 框架关联关系映射
		
		在内存中产生多个对象时,对象之间存在哪些关联关系?
			多对一,一对多,一对一,多对多
			
		面向对象模型中描述对象之间的关系存在单向和双向两种;	
		
	2) 一对多(班级和学生)单向关联
		
		2-1) 从对象模型角度	
		
			在一的一端增加对的一端集合的引用
				
		2-2) 从关系模型角度
			
			在学生表中增加对班级表一个引用的外键.	
				
		2-3) 从映射模型角度	
		
			<!-- 描述一对多关联关系映射:外键映射 -->
			<set name="students">
				<key>
					<column name="cid"></column>
				</key>
				<one-to-many class="com.bjpowernode.hibernate.pojo.Student"/>
			</set>
			
	3) 一对多关联,由一的一端来维护外键,那么,在维护外键时,往往会执行多余的update语句.影响系统性能.
	
		怎么解决性能问题?
			
		将一对多,映射成双向一对多和多对一;将外键的维护由一的一端委托给多的一端来维护;	
		
		3-1) 班级放弃维护外键,将外键维护委托给多的一端
			<!-- 描述一对多关联关系映射:外键映射 
				inverse="true" 表示当前端放弃维护外键了.
			-->
			<set name="students" inverse="true"></set>
			
		3-2) 建立多对一关联
		
			3-2-1) 在学生类中增加对班级对象引用
					private Classes classes ; //建立多对一关联
					get/set...
			
			3-2-2) 在学生映射配置中增加多对一标签映射
					<many-to-one name="classes" column="cid"></many-to-one>
				
	4) 一对多一般都是需要映射成双向一对多和多对一的,因为,一对多时,一的一端来维护外键,存在缺陷.

* 自关联

	1) 框架关联关系映射
		
		在内存中产生多个对象时,对象之间存在哪些关联关系?
			多对一,一对多,一对一,多对多
			
		面向对象模型中描述对象之间的关系存在单向和双向两种;	
		
	2) 自关联(栏目)
		
		 自关联是特殊的一对多和多对一映射.
		
		2-1) 从对象模型角度	
		
			private Set<NewsColumn> newsColumnSet = new HashSet<NewsColumn>(); //一对多
	
			private NewsColumn parentNesColumn ; //多对一
			
			get/set
				
		2-2) 从关系模型角度
			
			create table newsColumn(
				id int primary key auto_increment,
				title varchar(64) not null,
				content varchar(128),
				pid int
			);
			
			外键不能设置非空约束.
				
		2-3) 从映射模型角度	
		
			<many-to-one name="parentNesColumn" column="pid"></many-to-one>
		
			<set name="newsColumnSet" inverse="true">
				<key>
					<column name="pid"></column>
				</key>
				<one-to-many class="com.bjpowernode.hibernate.pojo.NewsColumn"/>
			</set>
* 多对多

	1) 框架关联关系映射
		
		在内存中产生多个对象时,对象之间存在哪些关联关系?
			多对一,一对多,一对一,多对多
			
		面向对象模型中描述对象之间的关系存在单向和双向两种;	
		
	2) 多对多(学生和课程) 双向关联
		
		2-1) 从对象模型角度	
		
			在多对多的两端分别增加对端集合类型引用.
				
		2-2) 从关系模型角度
			
			多对多需要通过一张中间表来描述数据的多对多关系.
			中间表应该含有两个外键,分别引用多对多两端表的主键;
			两个外键又作为联合主键,表示数据唯一性. 两个外键不能为null
				
		2-3) 从映射模型角度
		
			学生映射文件:
				
				<set name="courseSet" table="p_student_course">
					<key>
						<column name="sid"></column>
					</key>
					<many-to-many class="com.bjpowernode.hibernate.pojo.Course" column="cid"></many-to-many>
				</set>	
				
			课程映射文件:
				<set name="studentSet" table="p_student_course">
					<key>
						<column name="cid"></column>
					</key>
					<many-to-many class="com.bjpowernode.hibernate.pojo.Student" column="sid"></many-to-many>
				</set>	
		
	3) 多对多映射注意问题
	
		3-1) 默认情况下多对多的两端都来维护中间表数据,会出现联合主键重复;	
				解决办法,在其中一端来设置 inverse="true"
				
		3-2) 多对多映射级联不能取值detele和all
		
		3-3) 多对多映射时,如果中间表除了两个外键,还有额外的字段时,要映射成两个(双向)多对一/一对多.		

6 延迟加载
---------
	1) 什么是延迟加载?
	
		当查询数据时,不是立即查询数据库,而是返回一个代理对象,当访问代理对象的非主键属性值时,
		才会发送查询语句,到数据库中加载数据.
		
	2) 为什么使用延迟加载?
		
		延迟加载是为了提高系统性能.	
		因为有时数据使用不到,查询出来就是冗余的,浪费内存.
		
	3) 	延迟加载功能是如何实现的?
		
		采用动态代理机制实现的.(基于继承的)
		Hibernate3版本:Cglib动态代理
		Hibernate4版本:Javassist动态代理
		
	4) 延迟加载功能引用在哪些场合?
	
		4-1) 查询当前对象:load()
		
		4-2) 查询关联对象:多对一,一对多,多对多...
		
	5) 延迟加载应用时配置有哪些?
			
		5-1) <class>标签: load()
				lazy : 延迟加载
					true : 支持延迟加载功能,默认值
					false : 禁用延迟加载功能(不推荐)
					
		5-2) <many-to-one>标签: 多对一,一对一	
				lazy : 表示对关联数据采用延迟加载	
				取值:
					proxy : 支持延迟加载,默认值,采用代理方式实现的.
					no-proxy : 表示不采用代理方式实现延迟加载.(了解)
					false : 禁用延迟加载,不推荐	
					
		5-3) <set>标签:一对多,多对多
			
			lazy : 延迟加载
					表示对多的一端集合数据采用延迟加载
				取值:
					true : 支持延迟加载,默认值
					false : 禁用延迟加载
					extra : 支持延迟加载,在执行统计学生数量时,可以发送select count(*)语句;	
					
		5-4) <property>标签: 针对某一个属性延迟加载
			
				取值:
					false : 默认值,不支持延迟加载
					true : 延迟加载.需要采用字节码增强工具实现.							
		
	6) 如何禁用延迟加载
		
		通过标签的lazy="false"
		通过final修饰POJO类
		
		延迟加载功能是为了提高性能,推荐使用.
		
	7) 延迟加载功能各个标签上的lazy属性是否存在约束?
		
		标签之间存在层级关系,但是,延迟加载属性设置相互没有约束.		
		
	8) 延迟加载有效期?
		
		延迟加载功能不是一直有效的,一旦session.close();那么,延迟加载功能就失效了.
		如果session关闭后,再访问延迟加载功能产生的代理对象的非主键属性值时,就会抛延迟加载异常(no session异常).	
		
		希望采用延迟加载,那么,如何解决有效期问题?
			只需要保证利用延迟加载功能后再关闭session就可以了.(延迟Session关闭)
			
		采用OpenSessionInView模式:
		
			ThreadLocal<Session> + Filter/Interceptor


7 缓存
-------
	1) Hibernate框架的缓存
	
		如果频繁查询相同数据时,一般将第一次查询数据存放在缓存中,再次查询数据时,判断缓存中是否存在,
		如果存在,就可以利用缓存来获取数据,从而提高数据访问效率.
		
		减少与数据库频繁交互.
		
	2) Hibernate框架提供一级缓存和二级缓存机制.
	
		2-1) 一级缓存(Session缓存)
		
			当Session对象被创建了,一级缓存就存在了.如果Session被销毁了,那么,缓存失效了.
			Session对象属于请求范围,所以,作用不是很大,主要是Hibernate用一级缓存来管理持久化状态的对象;
			一个请求对应一个Session,一个Session对应一个Transaction.
			
			2-1-1) get()
					
					支持缓存的
					利用缓存的
					
			2-1-2) 一级缓存溢出问题
				/**
					java.lang.OutOfMemoryError: Java heap space
						at com.bjpowernode.hibernate.pojo.User.<init>(User.java:14)
						at com.bjpowernode.hibernate.dao.UserDAOTest.testSave1(UserDAOTest.java:108)
						at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
				 */	
				 
		2-2) 二级缓存(SessionFactory缓存)		
		
			一级缓存范围太小,利用率不高,需要提供更大级别缓存,可以在不同请求间共享数据;
			可以利用二级缓存,是应用级别的缓存.SessionFactory对象被创建,可以初始化二级缓存了.
			SessionFactory被销毁了,二级缓存失效了.
			SessionFactory对象在整个应用中只被创建一次,可以创建多个Session,所以二级缓存中数据时可以被多个session对象共享;
		
			一级缓存是自带的;
			二级缓存一般会采用第三方缓存组件,往往需要做配置,来启用二级缓存;
				Hibernate框架自己也提供二级缓存组件,是通过Hashtable实现的,比较简单,适合于练习使用,不适合生产环境.
		
			2-2-1) 配置第三方提供缓存组件(ehcache)
					拷贝jar包:
						ehcache-core-2.4.3.jar
						hibernate-ehcache-4.2.17.Final.jar
						slf4j-api-1.6.1.jar	
						ehcache.xml
						
						支持内存缓存数据,也支持硬盘缓存数据;	
										
			2-2-2) 启用二级缓存		
 					<property name="hibernate.cache.use_second_level_cache">true</property>
		
			2-2-3) 让框架识别二级缓存组件
					<property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
					
			2-2-4) 设置需要使用二级缓存的对象
				
					<!-- 设置哪些对象需要使用二级缓存 
						usage="read-only" 放置到二级缓存中的数据时只读的,不能进行修改.
					-->
					<class-cache usage="read-only" class="com.bjpowernode.hibernate.pojo.User"/>
		
		
* 缓存查询数据过程:
 
	当查询数据时,先从一级缓存中查找
		一级缓存中有,则直接获取利用	
		一级缓存中没有,判断是否启用二级缓存是否启用:
			如果启用,从二级缓存中查找数据:
				如果没有,查询数据库;
				如果有,则直接获取利用;
		将查询结果放置一级缓存和二级缓存中;给下次访问利用;	
		
		
* 缓存使用注意问题:

	1. 频繁访问的数据
	2. 缓存中数据不要经常修改(会降低效率)
	3. 缓存数据量控制一定大小
	4. 缓存中数据不应该被第三方修改
	5. 安全性要求高的数据不要使用缓存.例如:财务数据

* 查询缓存

	1) Hibernate框架的查询缓存
	
		1-1) list()
		
			支持缓存.
			默认自己不利用缓存.
			
		1-2) 如果希望list()方法也利用缓存,那么需要配置查询缓存.
		
			1-2-1) 利用第三方缓存组件(ehcache)	
			
					ehcache-core-2.4.3.jar
					hibernate-ehcache-4.2.17.Final.jar
					slf4j-api-1.6.1.jar			
					
					ehcache.xml
					
			1-2-1) 启用查询缓存
					查询缓存一般是要和二级缓存一起使用的,否则,查询缓存利用率不高.
					
					需要同时启用二级缓存和查询缓存;
					<property name="hibernate.cache.use_second_level_cache">true</property>
					<property name="hibernate.cache.use_query_cache">true</property>
					
			1-2-2) 让框架识别缓存组件				
				<property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
				
			1-2-3) 将需要利用缓存对象存放在二级缓存中.
				
				<!-- 设置哪些对象需要使用二级缓存 
					usage="read-only" 放置到二级缓存中的数据时只读的,不能进行修改.
				-->
				<class-cache usage="read-only" class="com.bjpowernode.hibernate.pojo.User"/>	
				
			1-2-4) 在程序中,需要启用查询缓存设置.
				
				Query query = session.createQuery("select u.name from User u where u.id<=?");
				query.setCacheable(true); //设置list方法启用查询缓存;								
				
	2) iterate()
	
		N+1条语句.
		
		实体查询:
			至少发送一条语句,只查询id
		属性查询:
			不支持缓存.	
			
		一般要结合支持缓存的方法一起使用,效率才会高;
			比如:
				先通过list()方法查询数据,将数据存放到缓存中;再次查询时,通过iterate()方法查询数据,可以利用缓存;		


9 Hibernate检索方式
-------------------
	Hibernate框架检索方式

	1) 如何查询某一个对象
	
		1-1) get()	主键查询
		
			返回结果:
				POJO对象或null(非空判断)
		
		1-2) load() 主键查询
		
			支持延迟加载功能.
			
			返回结果:
				代理对象或异常或POJO对象(禁用)
				
	2) 根据当前对象查询关联对象
			一般是采用延迟加载(lazy="proxy"/lazy="true" fetch="select")或连接查询(fetch="join")			
				
	3) HQL (重点)
	
		Hiberante Query Language   是一个面向对象的查询语言.Hibernate框架提供的.
		对SQL进行封装,语法类似于SQL语句,最终HQL需要翻译课程SQL才能进行数据库数据操作.	
		
			SQL : select u.username,u.age from t_user u where u.id=1
			HQL : from User u where u.id=1
					select u.name,u.age from User u where u.id=1
		
		支持查询:
			
			实体查询
			属性查询
			分页查询
			排序
			条件查询
			函数查询
			...
			
	4) QBC
			Query By Cretira  完全面向对象化查询.
			
				
	5) SQL
		
		非特殊场合下,不要使用.	

10 乐观锁/悲观锁
----------------

	1) Hibernate框架解决数据冲突问题
	
		并发事务对同一条数据进行操作,可能产生数据冲突问题.
		
		Hibernate框架通过悲观锁和乐观锁来解决数据冲突问题.
		
	2) 悲观锁
	
		悲观锁实质上就是指的数据库中的行级锁.	
		
		行级锁是数据库提供的一种锁定数据模式,控制数据的并发访问安全的一种机制.类似于Java中的线程同步机制;
		
			select ...   for update //表示对查询的数据进行锁定.
			
			事务隔离级别:1,2,4,8
			
			1    	未提交读
			2  		已提交读					Oracle
			4		重复读					MySQL
			8		序列化读(不可并发)	
			
			4   重复读	
				解决数据丢失,脏数据,不可重复读这些问题.
				在同一个事务内,不能读取到另外一个事务未提交或已提交事务的数据;
			
			级别				可能出现问题
			-------------------------------
			0				数据丢失    脏数据    不可重复读   幻读
			1				脏数据    不可重复读   幻读	
			2				不可重复读   幻读	
			4				幻读	
			8					
			
			重点:java.sql.Connection
			 int TRANSACTION_NONE             = 0;
			 int TRANSACTION_READ_UNCOMMITTED = 1;
			 int TRANSACTION_READ_COMMITTED   = 2;
			 int TRANSACTION_REPEATABLE_READ  = 4;
			 int TRANSACTION_SERIALIZABLE     = 8;
			
			
			JDBC中如何控制事务:
				conn.setAutoCommit(false);				
				conn.setTransactionIsolation(Connection.TRANSACTION_REPEATABLE_READ); //控制事务隔离级别
								
				conn.commit();				
				conn.rollback();
			
			Hibernate框架如何控制事务:
				session.beginTransaction();//conn.setAutoCommit(false);			
				
				session.getTransaction.commit();  //conn.commit();	
				
				session.getTransaction.rollback(); //conn.rollback();
			
		悲观锁是利用数据库行级锁来解决数据冲突问题;
			在执行查询时增加锁定模型.
			Account a = (Account)session.get(Account.class, 101,LockOptions.UPGRADE);	
	
	3) 乐观锁
	
		由于悲观锁类似线程同步机制,效率比较低;框架实现乐观锁机制.不是对数据进行锁定,而是在提交事务时对数据版本进行判断,
		如果版本变化,那么不允许提交,如果版本没有变化,那么可以提交;
		
		乐观锁可以通过不同方式实现:
			时间戳
			版本号(Hibernate)
			
		3-1) 需要设置乐观锁
			
			<class optimistic-lock="version">
			
		3-2) 在POJO类中增加版本属性
		
			private int version ; //类型是整数类型 				
			get/set方法
			
		3-3) 增加版本号属性映射配置
			
			<version name="version" column="version"></version>
			
		3-4) 重新创建表结构
			 版本号字段不能为null
			 
		3-5) 乐观锁代替悲观锁:
			去掉查询的锁定模式:LockOptions.UPGRADE
			
		框架来维护版本号的值,每一次数据修改,版本号字段值自动+1




























