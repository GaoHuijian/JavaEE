MySQL所有数据类型
==================
整数类型 范围

	tinyint 	-128 到 127
	smallint	 -32768 到 32767
	mediumint 	-8388608 到 8388607
	int 		-2147483648 到 2147483647
	bigint 		-9223372036864775808 到 9223372036854775807
	数类型有可选的 unsigned 属性，它不允许负数。范围为最小值为 0，最大值为上表中对应范围的最小值取
	绝对值后加上最大值。

整数类型 同义词

	tinyint 	int1
	smallint	int2
	mediumint 	int3， middleint
	int int，	int4
	bigint		int8

实数

	float		4字节
	double		8字节
	decimal

字符类型

	varchar
	char
	binary		二进制
	varbinary	二进制
	blob		二进制	大量数据  tinyblob smallblob blob mediublob longblob 
	text		字符		大量数据	 tinytext smalltext text mediumtext longtext
日期
	
	datetime
	timestamp
其他
	
	bit类型
	set类型

	enum