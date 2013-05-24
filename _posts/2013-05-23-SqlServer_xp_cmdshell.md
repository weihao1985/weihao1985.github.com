---
layout : post
category : SqlServer
tags : [SqlServer,xp_cmdsell,导出数据]
title : SqlServer用xp_cmdshell导出数据
---

SqlServer用xp_cmdshell导出数据 

	SQL Server的导出导入方式有：

	1.在SQL Server中提供了导入导出的界面操作。 
	2.在界面操作中又分【复制一个或多个表或视图的数据】和【编写查询以指定要传输的数据】两种模式，第一种是直接对表、视图进行全部字段、记录进行导出，而第二种就是可以通过SQL语句来控制导出导入的字段和行。
	3.使用 简单但有用的SQL脚本 中的【表复制】这里面的方法。
	4.再一种就是在命令行中使用bcp命令来导入导出数据，需要特别说明的是，这是对大数据量导入导出就好的办法。

	--整个表导出(out)
	bcp 数据库名.dbo.表名 out c:\table.txt -S"数据库实例" -U"用户" -P"密码" -c 

	--使用SQL语句导出(queryout)
	bcp "select * from 数据库名.dbo.表名" queryout c:\table.txt -S 数据库实例 -U"用户" -P"密码" -c

	--设置字段分隔符和行分隔符(-c -t"," -r"\n"),不想输入字段类型等请配合-c一起使用
	bcp "select * from 数据库名.dbo.表名" queryout c:\table.txt -S 数据库实例 -U"用户" -P"密码" -c -t"," -r"\n"

	--指定每批导入数据的行数、指定服务器发出或接收的每个网络数据包的字节数(-k -b5000 -a65535)
	bcp "select * from 数据库名.dbo.表名" queryout c:\table.txt -S 数据库实例 -U"用户" -P"密码" -c -t"," -r"\n" -k -b5000 -a65535

	--在查询分析器上执行(EXEC master..xp_cmdshell)
	EXEC master..xp_cmdshell 'bcp "select * from 数据库名.dbo.表名" queryout c:\table.txt -S 数据库实例 -U"用户" -P"密码" -c'

	--把SQL语句生成一个.sql文件，然后调用
	--注：路径的文件夹名称中间不能有空格
	exec master..xp_cmdshell 'osql -S 数据库实例 -U 用户 -P 密码 -i    C:\cmdshellTest.sql'  

	--将数据导入到currency表中
	EXEC master..xp_cmdshell 'bcp 数据库名.dbo.表名 in c:\table.txt -c -T'
	--导入数据也同样可以使用-F和-L选项来选择导入数据的记录行。
	EXEC master..xp_cmdshell 'bcp 数据库名.dbo.表名 in c:\table.txt -c -F 10 -L 13 -T' 

	在使用命令xp_cmdshell的时候需要设置权限：


	/*MSsql2005 如何启用xp_cmdshell
	默认情况下,sql server2005安装完后,xp_cmdshell是禁用的(可能是安全考虑),如果要使用它,可按以下步骤
	*/
	-- 允许配置高级选项
	EXEC sp_configure 'show advanced options', 1
	GO
	-- 重新配置
	RECONFIGURE
	GO
	-- 启用xp_cmdshell
	EXEC sp_configure 'xp_cmdshell', 1
	GO
	--重新配置
	RECONFIGURE
	GO

	--执行想要的xp_cmdshell语句
	Exec xp_cmdshell 'query user'
	GO

	--用完后,要记得将xp_cmdshell禁用(出于安全考虑)
	-- 允许配置高级选项
	EXEC sp_configure 'show advanced options', 1
	GO
	-- 重新配置
	RECONFIGURE
	GO
	-- 禁用xp_cmdshell
	EXEC sp_configure 'xp_cmdshell', 0
	GO
	--重新配置
	RECONFIGURE
	GO

	遇见的错误：
	1、发生以下错误：
	[Error][Microsoft][Native]Error = [Microsoft][SQL Native Client]无法打开 BCP 主数据文件

	使用如下命令：
	EXEC xp_cmdshell 'ECHO %USERDOMAIN%\%USERNAME%'
	返回 ：NT AUTHORITY\NETWORK SERVICE

	然后在配置管理器（configuration manager)里面的SQL server2005服务里打开，看到登陆内置账号为Network service,
	改成local system问题解决。

	2、SQLState = 22018, NativeError = 0
	Error = [Microsoft][SQL Native Client]对于造型说明无效的字符值

	如果是表与表之间的数据导入，可用 -N, 或者 -w, 而不要用 -c
	用-c的话, 如果导出的某个列中的数据中包含分隔符, 则会导致你 bcp 导入的时候失败
	-N 或者 -w 不会有这个问题 
	bcp用法: bcp {dbtable | query} {in | out | queryout | format} 数据文件
	  [-m 最大错误数]             [-f 格式化文件]         [-e 错误文件]
	  [-F 首行]                       [-L 末行]                  [-b 批大小]
	  [-n 本机类型]                 [-c 字符类型]            [-w 宽字符类型]
	  [-N 将非文本保持为本机类型] [-V 文件格式版本]     [-q 带引号的标识符]
	  [-C 代码页说明符]           [-t 字段终止符]       [-r 行终止符]
	  [-i 输入文件]                   [-o 输出文件]         [-a 数据包大小]
	  [-S 服务器名称]              [-U 用户名]            [-P 密码]
	  [-T 可信连接]                  [-v 版本]                [-R 允许使用区域设置]
	  [-k 保留空值]                  [-E 保留标识值]
	  [-h"加载提示"]                 [-x 生成 xml 格式化文件]
	其它：
	新建查询－>输入SQL查询语句，执行得到需要的结果，在查询结果栏点击鼠标右键－>将结果另存为 
	导出文件(*csv)即可导出为 逗号分割的excel文件。 
	此时，由于没有分列，所以应先新建一个空白的Excel文档，然后选择打开刚才保存的的文件; 
	然后它会出现文本导入步骤，选中分割符号，下一步，选中 逗号，预览区域正常，然后在下一步和完成，保存为excel文件
