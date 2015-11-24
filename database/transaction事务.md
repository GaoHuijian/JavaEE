Transaction
============

JDBC
----
* 有默认事务-->默认提交
	* execute方法执行,立刻处理,抛出异常,回滚事务
* 手动事务
		connection.setcommit(false);
		commit();

Hibernate
---------
* 默认事务-->默认回滚
* 有事务,ＪＰＡ　和　ＪＤＢＣ事务

* 手动事务


Spring
-------



MyBatis
------
* 默认事务-->默认回滚
	* 在回话关闭时,session.close()执行的时候,会检查事务

* 手动事务
	* 成功提交 session.commit();
	* 异常自动回滚 /最好: session.rollback();