---
layout: post
title:  "Windows上编译Boost库"
date:   2024-08-31 20:08:08 +0800
categories: cpp 
tags: cmake
---

在Windows上编译Boost库

- 生成b2编译程序
```bash

.\bootstrap.bat

```
- 安装b2(可选)
```bash

.\b2.exe install --prxfix=<Boost.Build installation path>
# 可以添加到环境变量使用
```


- 用b2编译boost
```bash
 C:\libs\Boost_build\b2.exe --build-dir=D:\Libraries\boost_1_85_0\build_vc141 toolset=msvc-14.1 --build-type=complete install --prefix=C:\libs\Boost_1_85_vc141
#也可用stage代替install指令，但相应的指令也要改

```