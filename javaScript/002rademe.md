JS特性
-----
* 在JS中,我们可以动态的给一个类添加属性添加方法,新添加的属性和方法对这个类所有的对象都是有效的<通过prototype属性>
* 也可以动态的删除对象的某个属性,但是不会影响其他的对象<使用delete>
* 我们可以给一个对象动态的添加属性和方法,而不会影响其他的对象.

* 在Java中,一个类的属性可以是另外一个类,JS中也可以这样做.
* this在JS中有两种用法
	* 1,出现在HTML标记中,代表当前的HTML暴击,被当做函数调用的实参来使用
	2 ,声明类的时候this指当前对象.

void
----


在JS中使用自定义对象的过程:
1,声明类
2,用这个类创建对象
3,调用对象上面的属性和方法

JSON对象--->轻量级数据交互格式
-------
* 使用JSON可以简化自定义对象的使用过程,把第一步和第二步合二为一.
* 使用JSON对象的时候,对象的属性可以是一个string
* JSON可以很方便的组成复杂的数据结构


流程控制
-------
* if
* switch case
	+ 在Java中,switch的条件和一个int兼容的表达式或变量
	+ 在JS中switch的条件可以是任意类型的表达式或变量
* for in 类似于Java中的for each.
	+ 作用:遍历数组
	+ 作用:遍历对象的成员


eval()函数
----------
* 能执行string里的有效的JS代码
* //eval(str)

String引用类型
-------------
* String的方法,string都可以调用
* string实际上就是用String创建出来的一个具体的对象而已,只不过JS中把这个string当做基本类型
* anchor("xx") 用JS向页面输出一个锚点
* replace() 正则表达式替换
* split() 分割字符串
* substr() 返回只能长度的子串

* link()用JS向页面输出一个超链接
* number实际上就是用Number创建出来的一个具体对象
* boolean实际上就是Boolean创建出来的一个具体对象


JS中使用数组
-------------------
* 方式1:-------> var arr = new Array();
* 方式2:-------> var arr = new Array('A' ,'B');
* 方式3:-------> var arr = ['A', 'B'];
* JS中的数组:没有数据类型的限制,也没有长度的限制.
* join();
* sort(排序函数);

Date对象
-------
*　var date = new Date() //创建当前时间的日期对象

* setInter()
* on

正则表达式
------

BOM
---


DOM
---


* 控制页面控件对象的状态,防止用户出错,这叫页面控制逻辑

* JSON
	* 格式
	* JSON与XML比较
		* xml 配置文件
		* json 数据交换

* javaScript如何将json字符串转为json对象  : eval();











