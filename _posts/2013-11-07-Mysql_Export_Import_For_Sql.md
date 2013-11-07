---
layout : post
category : mysql导出和导入文件
tags : [mysql导出和导入文件,INTO OUTFILE,load data local infile]
title : mysql导出和导入文件
---

## mysql导出和导入文件 ##

使用SELECT Field1，Field2 INTO OUTFILE 以逗号分隔字段的方式将数据导出到一个文件中

	select * into OUTFILE 'c:\\user.txt' FIELDS TERMINATED BY ',' 
	OPTIONALLY ENCLOSED BY '"' LINES TRMINATED BY '\n' FROM t_user;

使用load data local infile 将导出的文件user.txt导入到相同结构的新表中

	load data local infile c:\\user.txt into table t_user character set gbk
	FIELDS TERMINATED BY ',' LINES TRMINATED by '\n';`

说明:

	FIELDS TERMINATED BY ',' 字段之间间隔用','分割
	OPTIONALLY ENCLOSED BY '"' 将字段中字符类型的字符串放在双引号中，数值类型不做操作
	LINES TERMINATED BY '\n' 行间隔用换行符分割
	导出和导入的文件全部为绝对路径目录，导出字段可以自定义。