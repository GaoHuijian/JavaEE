1.函数
------
* dual 表:只有一个字段,并且只有一行数据,dual表中数据在运行期间,是不能修改.测试函数用

* select length('') from dual 返回 0;
* select length(null) from dual 返回 null;
* 	case 
	when 
	then 
	else 
	end 结构体
2.表修改
--------
###表结构的修改
* 1,添加新字段  alter table 表名 add 字段名 字段类型
* 2,更改字段   alter table 表名 modify 字段名 新字段类型
* 3,删除字段   alter table 表名 drop 字段名
* 4,修改字段名  alter table 表名 change 旧字段名 新字段名 字段类型
###表中约束修改
* 非空约束 not null
* 唯一性约束 unique key
* 主键约束   foreign key
* 外键约束   primary key

	    外键约束满足条件:
		1,要求与当前多方表关联一方表中,必须存在一个主键约束
		2,要求多方表中外键字段的数据类型,必须与一方表中主键字段的数据类型完全相同.
        3,constraint 约束名 foreign key(外键字段) reference 外键主表(外键主表字段)


		mysql 对有些约束的修改比较麻烦，所以我们可以先删除，再添加
		alter table t_student drop foreign key fk_classes_id;
		alter table t_student add constraint fk_classes_id_1 foreign key(classes_id) references
		t_classes(classes_id) on update cascade;

3.索引
-----
采用二叉树形式保存数据,使得查询,遍历数据行的次数大大减少
* 1)添加索引	create unique index 索引名 on 表名(列名)
		
* 2)判断是否起作用    explain 语句-->select sal from emp where sal > 1500;
* 3)查看表拥有索引信息	 show index from emp;
* 4)何种表适合添加索引
	+ 1,表中的数据量巨大
	+ 2,表中的数据很少被修改
* 5)删除索引

		DROP INDEX index_name ON talbe_name
		ALTER TABLE table_name DROP INDEX index_name
		ALTER TABLE table_name DROP PRIMARY KEY




内置聚合函数
----------
* 聚合:对列中所有的数据进行统计
* MySQL聚合函数的分类:
	+ sum(列名称) : 计算当前列中所有数据相加之和  	* 只针对数字类型列进行运算
	+ max(列名称) : 计算当前列中最大的数据       * 只针对数字类型列进行运算
	+ min(列名称) : 计算当前列中最小的数据       * 只针对数字类型列进行运算
	+ count(*)   : 计算表中的总行数
	+ count(字段) : 计算表中当前非空字段的行数
	+ avg(列)    : 计算当期列中的平均值







