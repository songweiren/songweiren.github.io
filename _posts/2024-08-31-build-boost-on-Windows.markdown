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
~~- 安装b2(可选)~~


~~.\b2.exe install --prxfix=<Boost.Build installation path>~~
~~# 可以添加到环境变量使用~~



- 用b2编译boost

```bash
.\b2.exe --build-dir=D:\Libraries\boost_1_85_0\build_vc141 toolset=msvc-14.1 --build-type=complete install --prefix=C:\libs\Boost_1_85_vc141
#也可用stage代替install指令，但相应的指令也要改

```
--build-dir：编译过程中产生的临时文件存放目录
toolset：工具集
--prefix：最后生成的库存放目录
--build-type：编译类型，complete可以全部编译所有可能