SAX解析
========
原理:基于事件型的解析方式,不会讲整个xml文件全部加载到内存中,它是顺序解析;
从左到右,从上到下.例如:在遇到开始节点的时候,表示发生的特殊的事件,会交给
专门的代码进行处理.

优点:不会导致内存溢出

缺点:不够灵活,解析过去的节点不能重复进行解析,除非从头开始
	但是这种情况不太严重,因为我们在解析的时候会结合xpath,xpath的作用,可以快速定位xml文件
	中元素的位置.