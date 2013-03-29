---
layout : post
category : Jquery
tags : [jquery,empty,remove]
title : jquery中用empty，remove删除 
---

jquery中用empty，remove删除 

 <pre>
 <html>
	<head>
	<meta charset="utf-8"/>
	<script type="text/javascript" src="../script/jquery-1.4.2.min.js"></script>
	<script>
	/*删除节点*/
	$(function(){
	//1,remove()移除 删除根据指定的参数删除
	$("ul li").remove("li[name=orange]");
	//$("ul li").remove("li[name=melon]");
	//如果没有指定参数,表示都删除
	//$("ul li").remove();
	//$("ul li").remove();
	//2,empty()清空,只清除内容
	//$("ul li").empty("li[title=橘子]");
	$("ul li[title=橘子]").empty();
	});
	</script>
	</head>
	<body>
	<p title="水果">你最喜欢的水果</p>
	<ul>
	<li title="橘子">橘子</li>
	<li>苹果</li>
	<li>梨</li>
	<li name="orange">橙子</li>
	<li name="melon">西瓜</li>
	</ul>
	</body>
</html>
 </pre>
