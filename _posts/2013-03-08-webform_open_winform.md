---
layout : post
category : net
tags : [webform调用winform]
title : 简单的C#.net webform调用winform
---

简单的C#.net webform调用winform

  先创建一个"Windows 应用程序"，然后直接生成、发布、安装该项目。
  
  再创建一个网站，在Default.aspx.cs的Page_Load方法中加入：

  string path = "应用程序生成的目录";

  System.Diagnostics.Process.Start(@path+"setup.exe");
