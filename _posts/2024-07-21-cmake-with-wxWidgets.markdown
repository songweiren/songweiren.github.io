---
layout: post
title:  "Windows上使用CMake编译使用wxWidgets的程序"
date:   2024-07-21 20:08:08 +0800
categories: cpp 
tags: cmake
---

在Windows上CMake编译使用wxWidgets开发的Win32程序使用的CMakeLists文件
```bash
cmake_minimum_required(VERSION 3.0)

project(wxWidgetsDemoDiplay)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
#设置find_package查找路径
set(CMAKE_PREFIX_PATH "C:/libs")
#静态编译链接wxWidgets库
set(wxWidgets_USE_STATIC TRUE)


find_package(wxWidgets REQUIRED COMPONENTS net core base adv jpeg tiff png zlib regex expat)
if(wxWidgets_USE_FILE) # not defined in CONFIG mode
    include(${wxWidgets_USE_FILE})
endif()
#遍历源文件
file(GLOB src_files *.cpp)
#增加图标
set(DEFAULT_RC_FILE "sample.rc")
list(APPEND src_files ${DEFAULT_RC_FILE}) 
#需要增加win32，否则无法找到main函数入口点
add_executable(${PROJECT_NAME} WIN32 ${src_files})
#wx_add_sample(WidgetsApp ${src_files})
target_link_libraries(${PROJECT_NAME} ${wxWidgets_LIBRARIES})



```
