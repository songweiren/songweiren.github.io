---
layout: post
title:  "c#读取Excel"
date:   2018-06-01 22:49:52 +0800
categories: C#
tags: C_Sharp
---
1、下载微软官方提供的操作Excel的dll，Microsoft.Office.Interop.Excel.dll

2、添加该dll到引用

3、直接上代码

```C#
using System.Reflection;
using Excel = Microsoft.Office.Interop.Excel;
using System.Xml;
using System.IO;
using System.Collections;
namespace ReadExcel
{
    class Program
    {
 
        static void Main(string[] args)
        {
            string excelFileName="test.xls"; 
            object miss = Missing.Value; 
            Excel.Application ea = new Excel.Application(); 
            Excel.Workbook ew = ea.Workbooks.Open(base_path + @"\" + excelFileName, miss, miss, miss, miss, miss, miss); //选择sheet1 
            Excel.Worksheet es = (Excel.Worksheet)ew.Worksheets[1]; // 选定读取范围 
            Excel.Range er = es.get_Range("A2", "K448"); //定义二位数组存储文本 
            string[,] myparam = new string[447, 12]; 
            for (int i = 1; i < 448; i++) 
            { 
              for (int j = 1; j < 13; j++) 
              {
                 myparam[i - 1, j - 1] = ((Excel.Range)er.Cells[i, j]).Text.ToString(); 
                 Console.Write(myparam[i - 1, j - 1] + ","); } Console.WriteLine(); // 
                 progressBar.Dispaly((Convert.ToInt32((i / (448 * 1.0)) * 100))); 
              } 
              ea.Workbooks.Close(); ea.Quit(); 
              System.Runtime.InteropServices.Marshal.ReleaseComObject(ea); 
              ea = null; 
              System.GC.Collect();
            }
      }
}
```