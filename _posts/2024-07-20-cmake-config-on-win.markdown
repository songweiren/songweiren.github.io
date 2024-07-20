---
layout: post
title:  "CMake Confign on Win"
date:   2024-07-20 20:49:52 +0800
categories: cpp 
tags: cmake
---

在Windows上CMake编译Win32程序
使用MinGW的编译命令为：
``` bash
cmake .. -G 'MinGW Makefiles' -DCMAKE_BUILD_TYPE=Release
cmake --build .
```
可以成功编译出Release版本。
MSVC的编译命令为：
```bash
cmake .. -G 'Visual Studio 16 2019' '-A Win32' -DCMAKE_BUILD_TYPE=Release
cmake --build .
```
只能编译出Debug版本，Release版本需要手动打开VS工程编译。
生成器是VS的情况下，生成版本只能在编译期配置，应将命令改为：
```bash
cmake .. -G 'Visual Studio 16 2019' '-A Win32'
cmake --build . --config Release
```
ref
https://github.com/OpenKinect/libfreenect/issues/444