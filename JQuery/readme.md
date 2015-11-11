jQuery
=======
* 优点:体积小,功能大,插件多,兼容性好,dom操作简单
* 功能:dom操作  ajax交互

* 选择器

* DOM文档加载完成后执行
	* $(document).ready();
----------------------
* 返回值:jQuery  jQuery(callback)
			$(document).ready(function() {
			 //代码
			}
			)

*可简写为:
			$(function(){
			//代码		
			})

* $.get $.post 封装了 $.ajax

* $代表什么意思? jQuery对象