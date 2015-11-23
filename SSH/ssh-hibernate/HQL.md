HQL

Hibernate Query Language 是一个面向对象的查询语言.Hibernate框架提供的.
对sql进行封装,语法类似于Sql语句,最终HQL需要翻译成SQL才能进行数据库数据操作.

lazy	:proxy 	:支持延迟加载,默认值
    	:no-proxy:	:不采用代理方式实现延时加载.
	:false	:禁用延迟加载

fetch:抓取策略
	表示采用什么样语句,查询关联数据.
	select :采用多条语句的方式进行关联数据查询.
	join   :表示采用连接语句的方式进行查询. 默认采用左外联接查询.
 
如果采用连接查询,延迟加载就失效了.	
	
不支持select * from User
from User
select U from User as U


传递参数 从0开始
Query query = session.createQuery("from User u where u.name like ?");

Query query = session.createQuery("delete from User u where u.id in (:ids)"); //   :ids  表示定义一个参数名称(名称是自定义的)

query.setFirstResult(startIndex);
query.setMaxResults(pagesize);

