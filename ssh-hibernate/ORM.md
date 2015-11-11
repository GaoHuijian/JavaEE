@ManyToOne
----------

	多对一(学生和班级)单向关联		
		2-1) 从对象模型角度		
			在多的一端增加对一的一端的对象引用.				
		2-2) 从关系模型角度			
			在学生表中增加对班级表一个引用的外键.					
		2-3) 从映射模型角度		
			<!-- 多对一关系映射:外键映射 
			外键字段值,默认来自关联班级对象主键属性值;
			-->
			<many-to-one name="classes" column="cid" class="com.bjpowernode.hibernate.pojo.Classes"></many-to-one>	



@OneToMany
----------