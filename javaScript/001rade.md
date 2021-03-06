java强类型:
-----------
* (1)声明变量必须指明变量的类型
* (2) 变量的类型是固定,是不能改变的

JS弱类型
------
* (1)声明变量的时候不需要指明变量的类型(统一用var声明,甚至不用var都可以
* (2)变量的类型是可以改变的

JS中的数据类型
###一:基本类型

* 1,string类型---------------->可以使用"" ''
* 2,number类型--------------> JS不区分整形和浮点型,都是number类型
	+ Infinity NaN
	+ 任何涉及NaN的操作（例如NaN/10）都会返回NaN
	+ NaN与任何值都不相等，包括NaN本身
	+ isNaN()函数,该函数会尝试将参数转成一个数值,如果能得到有效的数值,则isNaN()返回false,否则返回true.
* 3,boolean类型------------>true / false
* 4,undefined类型---------->当声明一个变量直接赋值为unfefined或,变量为赋值时时unfefined类型的.
* JS会自动的把非boolern类型的表达式转换为boolean类型的表达式.
* boolearn类型转换的规则--------------->见图片.
* JS会把非number类型自动转换为number类型 或者使用Number()函数或者parseInt()/parseFloat()函数
	+ Number()转换规则如下：
	+ 如果是Boolean值，true和false将分别被替换为1和0
	+ 如果是数字值，只是简单的传入和返回
	+ 如果是null值，返回0
	+ 如果是undefined，返回NaN
	+ 如果是字符串，遵循下列规则：
	+ 如果字符串中只包含数字，则将其转换为十进制数值，即”1“会变成1，”123“会变成123，而”011“会变成11（前导的0被忽略）
	+ 如果字符串中包含有效的浮点格式，如”1.1“，则将其转换为对应的浮点数（同样，也会忽略前导0）
	+ 如果字符串中包含有效的十六进制格式，例如”0xf“，则将其转换为相同大小的十进制整数值
	+ 如果字符串是空的，则将其转换为0
	+ 如果字符串中包含除了上述格式之外的字符，则将其转换为NaN
	+ 如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString()方法，然后再依次按照前面的规则转换返回的字符串值
	+ Number()函数
	+ parseInt()/parseFloat()只能将string格式的数据转换为number类型
	+ parseInt()转换的结果是不带小数的,可以识别不同的进制0x十六进制,0xxxx二进制
	+ parseInt()可以把同一个string安照不同的进制来转换
	+ parseFloat()转换的结果是带有小数的


###二:引用类型

* 当用var来声明变量的时候,变量的类型是不确定的,也就是undefined,当我们给变量赋值的时候,变量的类型就会改变
* 在<script>标准中声明的变量成为全局变量,在这个页面中都是有效的.
* 在一个函数或代码块{}中用var声明的变量称为局部变量,只在它声明的{}里面有效.
* 在函数内部声明变量的时候,如果没有使用var,那么这个变量也是全局变量.
* 和Java中一样,当全局变量和局部变量名称重复的时候,在函数内部优先使用局部变量(就近原则).

JS函数
------
* JS中的函数是吧一组JS代码封装成一个整体,通过这种方式来提供代码的复用率

######声明函数的两种方式:--------------->声明有参数的函数,参数不能指定类型(连var都不能有)
* 方式1:  function 函数名称([参数列表]){}

* 方式2: 函数名称 = function([参数列表]){}
* 在声明函数的时候,不需要声明函数的返回类型,直接在方法中返回结果就可以了.
* 在JS调用的时候,实参和形参不一致,也不会出现错误.
* JS中没有方法重载的概念,在这里就近原则在发挥作用,所以,在JS中声明函数的时候,函数的名称不要相同.


* 失去焦点事件:blur
* 失去焦点事件句柄
* 得到焦点事件:onfocus
* 得到焦点事件句柄:
* 点击事件:onclerk
* this表示当前对象


JS引用类型
--------
* JS引用类型和Java中的引用类型用法一样,也是通过new来创建对象,然后调用对象上面的属性和方法
* JS中出了可以使用引用类之外,也可以和Java中一样,使用自定义的引用类型.
* JS使用自定义引用类型过程:
	+ 1,声明类.
 	+ 2,用声明的类创建对象.
	+ 3,调用对象上面的属性和方法.
*  JS中的函数也是一种引用类型
*由于JS中的类型是可变的,我们就需要一种手段来检查变量的类型,可以使用typeof函数
	+ typeof函数的用法
	+ typeof(xx) || typeof xx 可以不加小括号
	+ typeof() 返回值:string类型

string类型
----------
* JS中的string类型,是采用Unicode编码的字符序列,可以使用"",也可以是用'',
* string在JS中被当做基本类型,它上面也有属性,也有方法.

####把其他类型的数据转为string类型的不同方式
* 几乎任何类型都有toString()方法.<undefined和null没有toString()的方法>
* 可以使用string()函数,把任何类型的数据转换为string类型.
* 任何类型的数据遇到string,得到的结果都是string.

Object
-------
* JS中的引用类型和Java中的引用类型用法一样
* JS提供了多种引用类型,和Java中一样,js中也提供了Object类型,在JS中也存放着继承机制
* Object是所有类的共同基类,JS中也存在着方法覆盖的概念.
* constructor ------------------------->  输出对象的构造函数
* prototype   ------------------------->  可以使用prototype属性给一个类动态的添加属性,添加方法,新添加的属性和方法对这个类所有的对象都是有效的.


运算符规则
-------------
### "乘法 *"
* 
* 
### "加法 +"
* 当+两边都是number类型的时候,则执行算术运算
* 只要+有一边是string,则做连接string的操作
### "==" 
* 与Java相同的地方
	+ 对于基本类型来说,是比较连个基本类型的值时候相等
	+ 对于引用类型来说,是比较连个引用的地址是否相同,也就是两个引用是否指向一个对象.
* 与Java不同的地方
	+ Java中,==只能比较相同的类型
	+ JS中,当两个变量的类型不同的时候,JS首先会统一类型,然后再进行比较.
		+ 1)一个数字与一个字符串,字符串转换成数字之后,进行比较
		+ 2)true转换为1,false转换为0,进行比较
		+ 3)一个对象,数组,函数与一个数字或字符串, 对象,数组,函数转换为原始类型的值,然后进行比较.

### "==="
* 只有当两个变量的类型一致的时候,才会进行比较,如果两个变量的类型不一致,则直接返回false.

delete运算符
----------
* 可以删除对象的某个属性,但不会影响其他的对象
* 删除数组中的某个元素,但是不会影响其他元素和数组的长度
* 用在with语句中,删除对象的某个属性.	

with语句
--------
* 可以简化对象的访问方式


js文件
-----
* <script src="xxxxx.js"></script>










