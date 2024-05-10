---
layout: post
title:  "Python 将pdf文件转换为word文件"
date:   2024-05-10 20:49:52 +0800
categories: python  
tags: python
---

要在Python中将PDF转换为Word，可以使用pdf2docx库
``` bash
pip install pdf2docx
```
然后，使用以下代码将PDF文件转换为Word文档：
```python
from pdf2docx import Converter
 
# PDF文件路径
pdf_file = 'example.pdf'
 
# 输出的Word文档路径
docx_file = 'example.docx'
 
# 创建一个转换器
cv = Converter(pdf_file)
 
# 转换第一页
cv.convert(docx_file, start=0, end=1)
 
# 释放资源
cv.close()
```