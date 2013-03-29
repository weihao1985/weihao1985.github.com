---
layout : post
category : Jquery
tags : [jquery,append]
title : jquery中用append增加节点
---

jquery中用append增加节点 

<pre>
/*创建节点*/
	$(function(){
	//1,创建元素节点append
	var li = $("<li></li>");
	$("ul").append($li);
	//2,创建属性节点,创建元素节点的时候,写上属性就ok
	var title=$("<li title=apple></li>");
	$("ul").append($title);
	//3,创建文本节点,创建元素节点的时候,写上文本内容就ok
	var content=$("<li>芒果</li>");
	$("ul").append($content);
	//区别一下append()  和appendTo()
	//append() 父元素追加xxx为子元素
	//appendTo() 将匹配的元素追加到某个元素上
	var girl=$("<p title=桃>你好</p>");
	$girl.appendTo($("body"));
	}); 

	<p title="水果">你最喜欢的水果</p>
	<ul>
	<li title="橘子">橘子</li>
	<li>苹果</li>
	<li>梨</li>
	</ul>
</pre>