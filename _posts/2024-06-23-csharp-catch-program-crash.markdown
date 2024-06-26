---
layout: post
title:  "C# Winform程序奔溃捕获异常"
date:   2024-06-23 20:49:52 +0800
categories: C#  
tags: c#
---

在Program.cs中的Main方法中加入以下代码
```c#
  Application.EnableVisualStyles();
  Application.SetCompatibleTextRenderingDefault(false);
  Application.SetUnhandledExceptionMode(UnhandledExceptionMode.CatchException);
  Application.ThreadException += Application_ThreadException;
  AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException;

```
在Program.cs中实现这个两个异常捕获方法
```c#
private static void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e)
{
      
      using (FileStream efs = new FileStream("./error.txt", FileMode.Append))
      {
           byte[] data = System.Text.Encoding.Default.GetBytes("CurrentDomain_UnhandledException::" + e.ToString() + ",object:" + e.ExceptionObject.ToString());
            efs.Write(data, 0, data.Length);
      }
}
 
private static void Application_ThreadException(object sender, System.Threading.ThreadExceptionEventArgs e)
{
 
      using (FileStream efs = new FileStream("./error.txt", FileMode.Append))
      {
           byte[] data = System.Text.Encoding.Default.GetBytes("Application_ThreadException:" + e.ToString());
           efs.Write(data, 0, data.Length);
      }
}
```