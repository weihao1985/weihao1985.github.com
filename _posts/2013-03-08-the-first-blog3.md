---
layout : post
category : net
tags : [开启线程]
---

C#.net开启线程
  //创建一个线程来执行 Threadfunc这个方法
  Thread Threading = new Thread(new ThreadStart(Threadfunc));
  Threading.Start();

  //关闭线程
  Threading.Abort();