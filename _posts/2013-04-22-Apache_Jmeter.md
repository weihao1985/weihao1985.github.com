---
layout : post
category : 工具
tags : [Apache JMeter,测试工具]
title : Apache JMeter介绍及配置
---

##Apache JMeter介绍##

   Apache JMeter是Apache组织开发的基于Java的压力测试工具。用于对软件做压力测试，
   它最初被设计用于Web应用测试但后来扩展到其他测试领域。 它可以用于测试静态和动态资源例如静态文件、
   Java 小服务程序、CGI 脚本、Java 对象、数据库， FTP 服务器, 等等。

   JMeter 可以用于对服务器、网络或对象模拟巨大的负载，来在不同压力类别下测试它们的强度和分析整体
   性能。另外，JMeter能够对应用程序做功能/回归测试，通过创建带有断言的脚本来验证你的程序返回了你
   期望的结果。为了最大限度的灵活性，JMeter允许使用正则表达式创建断言。 Apache jmeter 可以用于对
   静态的和动态的资源（文件，Servlet，Perl脚本，java 对象，数据库和查询，FTP服务器等等）的性能进
   行测试。它可以用于对服务器，网络 或对象模拟繁重的负载来测试它们的强度或分析不同压力类型下的整
   体性能。你可以使用它做性能的图形分析
  或在大并发负载测试你的服务器/脚本/对象。

##Apache JMeter安装配置##

   1.下载apache-jmeter-2.6和jdk 1.6.0_10
   Apache Jmeter下载地址：http://jmeter.apache.org/download_jmeter.cgi 
   Java环境下载：http://www.oracle.com/technetwork/java/javase/downloads/index.html
   2.解压apache-jmeter-2.6，放入自己的目录
   3.配置环境变量
   我的电脑—>属性—>高级—>环境变量，设置以下环境变量：
   JAVA_HOME为{磁盘路径}\jdk1.6.0_10
   CLASSPATH为{磁盘路径}\jdk1.6.0_10\bin; {磁盘路径}\jdk1.6.0_10\lib\dt.jar;{磁盘路径}\jdk1.6.0_10\lib\tools.jar;
   PATH为{磁盘路径}\jdk1.6.0_10\bin
   4.运行apache-jmeter-2.6\bin\jmeter.bat,打开JMeter工具。