---
layout : post
category : SqlServer
tags : [SqlServer,xp_cmdsell,��������]
title : SqlServer��xp_cmdshell��������
---

SqlServer��xp_cmdshell�������� 

	SQL Server�ĵ������뷽ʽ�У�

	1.��SQL Server���ṩ�˵��뵼���Ľ�������� 
	2.�ڽ���������ַ֡�����һ�����������ͼ�����ݡ��͡���д��ѯ��ָ��Ҫ��������ݡ�����ģʽ����һ����ֱ�ӶԱ���ͼ����ȫ���ֶΡ���¼���е��������ڶ��־��ǿ���ͨ��SQL��������Ƶ���������ֶκ��С�
	3.ʹ�� �򵥵����õ�SQL�ű� �еġ����ơ�������ķ�����
	4.��һ�־�������������ʹ��bcp���������뵼�����ݣ���Ҫ�ر�˵�����ǣ����ǶԴ����������뵼���ͺõİ취��

	--��������(out)
	bcp ���ݿ���.dbo.���� out c:\table.txt -S"���ݿ�ʵ��" -U"�û�" -P"����" -c 

	--ʹ��SQL��䵼��(queryout)
	bcp "select * from ���ݿ���.dbo.����" queryout c:\table.txt -S ���ݿ�ʵ�� -U"�û�" -P"����" -c

	--�����ֶηָ������зָ���(-c -t"," -r"\n"),���������ֶ����͵������-cһ��ʹ��
	bcp "select * from ���ݿ���.dbo.����" queryout c:\table.txt -S ���ݿ�ʵ�� -U"�û�" -P"����" -c -t"," -r"\n"

	--ָ��ÿ���������ݵ�������ָ����������������յ�ÿ���������ݰ����ֽ���(-k -b5000 -a65535)
	bcp "select * from ���ݿ���.dbo.����" queryout c:\table.txt -S ���ݿ�ʵ�� -U"�û�" -P"����" -c -t"," -r"\n" -k -b5000 -a65535

	--�ڲ�ѯ��������ִ��(EXEC master..xp_cmdshell)
	EXEC master..xp_cmdshell 'bcp "select * from ���ݿ���.dbo.����" queryout c:\table.txt -S ���ݿ�ʵ�� -U"�û�" -P"����" -c'

	--��SQL�������һ��.sql�ļ���Ȼ�����
	--ע��·�����ļ��������м䲻���пո�
	exec master..xp_cmdshell 'osql -S ���ݿ�ʵ�� -U �û� -P ���� -i    C:\cmdshellTest.sql'  

	--�����ݵ��뵽currency����
	EXEC master..xp_cmdshell 'bcp ���ݿ���.dbo.���� in c:\table.txt -c -T'
	--��������Ҳͬ������ʹ��-F��-Lѡ����ѡ�������ݵļ�¼�С�
	EXEC master..xp_cmdshell 'bcp ���ݿ���.dbo.���� in c:\table.txt -c -F 10 -L 13 -T' 

	��ʹ������xp_cmdshell��ʱ����Ҫ����Ȩ�ޣ�


	/*MSsql2005 �������xp_cmdshell
	Ĭ�������,sql server2005��װ���,xp_cmdshell�ǽ��õ�(�����ǰ�ȫ����),���Ҫʹ����,�ɰ����²���
	*/
	-- �������ø߼�ѡ��
	EXEC sp_configure 'show advanced options', 1
	GO
	-- ��������
	RECONFIGURE
	GO
	-- ����xp_cmdshell
	EXEC sp_configure 'xp_cmdshell', 1
	GO
	--��������
	RECONFIGURE
	GO

	--ִ����Ҫ��xp_cmdshell���
	Exec xp_cmdshell 'query user'
	GO

	--�����,Ҫ�ǵý�xp_cmdshell����(���ڰ�ȫ����)
	-- �������ø߼�ѡ��
	EXEC sp_configure 'show advanced options', 1
	GO
	-- ��������
	RECONFIGURE
	GO
	-- ����xp_cmdshell
	EXEC sp_configure 'xp_cmdshell', 0
	GO
	--��������
	RECONFIGURE
	GO

	�����Ĵ���
	1���������´���
	[Error][Microsoft][Native]Error = [Microsoft][SQL Native Client]�޷��� BCP �������ļ�

	ʹ���������
	EXEC xp_cmdshell 'ECHO %USERDOMAIN%\%USERNAME%'
	���� ��NT AUTHORITY\NETWORK SERVICE

	Ȼ�������ù�������configuration manager)�����SQL server2005������򿪣�������½�����˺�ΪNetwork service,
	�ĳ�local system��������

	2��SQLState = 22018, NativeError = 0
	Error = [Microsoft][SQL Native Client]��������˵����Ч���ַ�ֵ

	����Ǳ����֮������ݵ��룬���� -N, ���� -w, ����Ҫ�� -c
	��-c�Ļ�, ���������ĳ�����е������а����ָ���, ��ᵼ���� bcp �����ʱ��ʧ��
	-N ���� -w ������������� 
	bcp�÷�: bcp {dbtable | query} {in | out | queryout | format} �����ļ�
	  [-m ��������]             [-f ��ʽ���ļ�]         [-e �����ļ�]
	  [-F ����]                       [-L ĩ��]                  [-b ����С]
	  [-n ��������]                 [-c �ַ�����]            [-w ���ַ�����]
	  [-N �����ı�����Ϊ��������] [-V �ļ���ʽ�汾]     [-q �����ŵı�ʶ��]
	  [-C ����ҳ˵����]           [-t �ֶ���ֹ��]       [-r ����ֹ��]
	  [-i �����ļ�]                   [-o ����ļ�]         [-a ���ݰ���С]
	  [-S ����������]              [-U �û���]            [-P ����]
	  [-T ��������]                  [-v �汾]                [-R ����ʹ����������]
	  [-k ������ֵ]                  [-E ������ʶֵ]
	  [-h"������ʾ"]                 [-x ���� xml ��ʽ���ļ�]
	������
	�½���ѯ��>����SQL��ѯ��䣬ִ�еõ���Ҫ�Ľ�����ڲ�ѯ������������Ҽ���>��������Ϊ 
	�����ļ�(*csv)���ɵ���Ϊ ���ŷָ��excel�ļ��� 
	��ʱ������û�з��У�����Ӧ���½�һ���հ׵�Excel�ĵ���Ȼ��ѡ��򿪸ղű���ĵ��ļ�; 
	Ȼ����������ı����벽�裬ѡ�зָ���ţ���һ����ѡ�� ���ţ�Ԥ������������Ȼ������һ������ɣ�����Ϊexcel�ļ�
