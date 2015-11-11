* 每一个JavaScript对象<除null>都是一个关联数组

创建对象
-------------
* 每个对象拥有三个相关的对象特性
		* 1.对象的原型 prototype
		* 2.对象的类 class
		* 3.对象的扩展标记 extensible flag

* 对象创建方式
	*  对象直接量---->由若干名/值对组成的映射表,名/值对中间用冒号分割,名/值对之间用逗号分割,整个映射表用花括号括起来
	*  关键字new----->new Object(); / new Array(); / new Date(); /new RegExp("js");
	*  Object.create()函数


* JavaScript的原型
	*  所有通过 对象直接量 创建的对象都具有同一个原型对象,并可以通过JavaScript代码Object.prototype获得原型对象的引用.
	*  通过关键字new和构造函数调用创建的对象的原型就是构造函数的prototype属性的值

