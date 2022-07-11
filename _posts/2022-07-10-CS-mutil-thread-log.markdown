---
layout: post
title:  "C# 轻量多线程log库"
date:   2022-07-11 18:49:52 +0800
categories: CSharp
tags: CSharp
---
最近总结了下之前写的C#log库，源码如下：分为两个文件

#### class LogInfo

```c#

//LogInfo.cs
public class LogInfo
    {
        /// 日志文件大小
        private int fileSize;
 
        /// 日志文件的路径
        private string fileLogPath;
 
        /// 日志文件的名称
        private string logFileName;
 
        /// 构造函数,初始化日志文件大小[2M]
 
        /// 可以使用相关属性对其进行更改.
 
        public LogInfo(string logName)
        {
 
            this.fileSize = 10 * 1024 * 1024;//2M
 
            //默认路径
 
            this.fileLogPath = Directory.GetCurrentDirectory();// + @"\log\";//路径，可以根据实际修改
 
            string nameTime = DateTime.Now.ToString("yyyy-MM-dd");
 
            this.logFileName = System.Environment.MachineName + nameTime + logName + ".txt";
        }
 
        /// 获取或设置定义日志文件大小
        public int FileSize
        {
            set
            {
                fileSize = value;
            }
            get
            {
                return fileSize;
            }
        }
        /// 获取或设置日志文件的路径
        public string FileLogPath
        {
            set
            {
                this.fileLogPath = value;
            }
            get
            {
                return this.fileLogPath;
            }
        }
        /// 获取或设置日志文件的名称
        public string LogFileName
        {
            set
            {
                this.logFileName = value;
            }
            get
            {
                return this.logFileName;
            }
 
        }
        /// <summary>
        /// 向指定目录中的指定的文件中追加日志文件
        /// <param name="Message">Message,要写入日志的内容</param>
        /// </summary>
        public void WriteLog(string Message)
        {
            this.m_WriteLog(this.logFileName, Message);
        }
 
 
 
        int cnt = 1;
        /// <summary>
        /// 向指定目录中的文件中追加日志文件,日志文件的名称将由传递的参数决定.
        /// <param name="LogFileName">日志文件的名称,如:mylog.txt ,如果没有自动创建,如果存在将追加写入日志</param>
        /// <param name="Message">Message,要写入日志的内容</param>
        /// </summary>
        private void m_WriteLog(string LogFileName, string Message)
        {
 
            //如果日志文件目录不存在,则创建
            if (!Directory.Exists(this.fileLogPath))
            {
                Directory.CreateDirectory(this.fileLogPath);
            }
            //static int cnt = 1;
            FileInfo finfo = new FileInfo(this.fileLogPath + LogFileName);
            if (finfo.Exists && finfo.Length > fileSize)
            {
                finfo.MoveTo(this.fileLogPath + LogFileName + "_bk_" + cnt.ToString());
            }
            try
            {
                FileStream fs = new FileStream(this.fileLogPath + LogFileName, FileMode.Append);
                StreamWriter strwriter = new StreamWriter(fs);
                try
                {
                    string tm = DateTime.Now.ToString("HH:mm:ss:fff");
                    strwriter.WriteLine("[" + tm + "]" + Message);
                    strwriter.Flush();
                }
                catch (Exception ee)
                {
                    //Console.WriteLine("日志文件写入失败信息:" + ee.ToString());
                    using (FileStream efs = new FileStream("./error.txt", FileMode.Append))
                    {
                        byte[] data = System.Text.Encoding.Default.GetBytes(ee.Message);
                        efs.Write(data, 0, data.Length);
                    }
                }
                finally
                {
                    strwriter.Close();
                    strwriter = null;
                    fs.Close();
                    fs = null;
                }
            }
            catch (Exception ee)
            {
                //Console.WriteLine("日志文件没有打开,详细信息如下:" + ee.ToString());
                using (FileStream efs = new FileStream("./error.txt", FileMode.Append))
                {
                    byte[] data = System.Text.Encoding.Default.GetBytes("日志文件没有打开,详细信息如下:" + ee.Message);
                    efs.Write(data, 0, data.Length);
                }
            }
        }
    }
```

#### **class LogMgr**

```c#

//LogMgr.cs 
   public class LogMgr
    {
        /// <summary>
        /// 初始化log文件名
        /// </summary>
        /// <param name="name"></param>
        public LogMgr(string name)
        {
            logInfo = new LogInfo(name);
            Start();
        }
        ~LogMgr()
        {
            Stop();
            if (th != null) th.Abort();
        }
 
        private readonly object mutex = new object();
        Queue<string> qlog = new Queue<string>();
        AutoResetEvent evt_log = new AutoResetEvent(false);
        LogInfo logInfo;// = new LogInfo("Info");
 
        void log2file()
        {
            while (isllog)
            {
                evt_log.WaitOne();
 
                while (qlog.Count > 0)
                {
                    lock (mutex)
                    {
                        logInfo.WriteLog(qlog.Dequeue());
                    }
                }
 
            }
        }
 
 
        /// <summary>
        /// Deprecated
        /// </summary>
        /// <param name="log"></param>
        public void FastLog(List<string> log)
        {
            lock (mutex)
            {
                foreach (var item in log)
                {
                    qlog.Enqueue(DateTime.Now.ToString("HH:mm:ss:fff") + " | " + "Info:" + item);
                }
            }
            evt_log.Set();//通知打印线程
        }
        Thread th;
        bool isllog = true;
 
        /// <summary>
        /// 停止log
        /// </summary>
        private void Stop()
        {
            isllog = false;
            if (th.ThreadState == ThreadState.Running || th.ThreadState == ThreadState.WaitSleepJoin)
                th.Abort();
 
        }
        
        /// <summary>
        /// 启动log服务线程
        /// </summary>
        void Start()
        {
            isllog = true;
            th = new Thread(new ThreadStart(log2file));
            th.IsBackground = true;
            th.Start();
 
        }
//public interface
        public void LogInfo(string str)
        {
            lock (mutex)
            {
                qlog.Enqueue(DateTime.Now.ToString("HH:mm:ss:fff") + " | " + "Info:" + str);
            }
            evt_log.Set();//通知打印线程
        }
        public void LogError(string str)
        {
            lock (mutex)
            {
                qlog.Enqueue(DateTime.Now.ToString("HH:mm:ss:fff") + " | " + "Error:" + str);
            }
            evt_log.Set();//通知打印线程
        }
        public void LogDebug(string str)
        {
            lock (mutex)
            {
                qlog.Enqueue(DateTime.Now.ToString("HH:mm:ss:fff") + " | " + "Debug:" + str);
            }
            evt_log.Set();//通知打印线程
        }
    }
```

使用

```c#
 LogMgr log = new LogMgr("info.txt");
 log.LogInfo("this is info log");
```

