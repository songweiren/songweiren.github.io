---
layout: post
title:  "C++ ShareMomory(Windows)"
date:   2022-07-10 18:49:52 +0800
categories: C/C++
tags: C_or_CPP
---
共享内存 (也叫内存映射文件) 主要是通过映射机制实现的 , Windows 下进程的地址空间在逻辑上是相互隔离的 , 但在物理上却是重叠的 ; 所谓的重叠是指同一块内存区域可能被多个进程同时使用 , 当调用 CreateFileMapping 创建命名的内存映射文件对象时 , Windows 即在物理内存申请一块指定大小的内存区域 , 返回文件映射对象的句柄 hMap ; 为了能够访问这块内存区域必须调用 MapViewOfFile 函数 , 促使 Windows 将此内存空间映射到进程的地址空间中 ; 当在其他进程访问这块内存区域时 , 则必须使用 OpenFileMapping 函数取得对象句柄 hMap , 并调用 MapViewOfFile 函数得到此内存空间的一个映射 , 这样系统就把同一块内存区域映射到了不同进程的地址空间中 , 从而达到共享内存的目的 , 代码如下 :

#### **进程 A 将数据写入到共享内存 :**

```c++
#include <iostream>
#include <Windows.h>
#include <string>

using namespace std;
#define BUF_SIZE 2048
string mem_name = "ShareMemory";
int main()
{
	char smData[] = "This is a share memory";

	// 创建共享文件句柄 
	HANDLE  mapFileHandle=CreateFileMapping(
		INVALID_HANDLE_VALUE,
		NULL,   // 默认安全级别
		PAGE_READWRITE,   // 可读可写
		0,   // 高位文件大小
		BUF_SIZE,   // 地位文件大小
		mem_name.data()   // 共享内存名称
	);

	// 映射缓存区视图 , 得到指向共享内存的指针
	LPVOID lpBase = MapViewOfFile(
		mapFileHandle,        // 共享内存的句柄
		FILE_MAP_ALL_ACCESS, // 可读写许可
		0,
		0,
		BUF_SIZE
	);
	memcpy(lpBase, smData, sizeof(smData));
	Sleep(20000);

	// 解除文件映射
	UnmapViewOfFile(lpBase);
	// 关闭内存映射文件对象句柄
	CloseHandle(mapFileHandle);
	system("pause");
	return 0;
}
```

#### **进程 B 获取共享内存中的数据**

```c++
#include <iostream>
#include <Windows.h>
#include <string>

using namespace std;
#define BUF_SIZE 2048
string mem_name = "ShareMemory";

int main()
{
	// 打开共享的文件对象
	HANDLE mapFileHandle = OpenFileMapping(FILE_MAP_ALL_ACCESS, NULL, mem_name.data());
	if (mapFileHandle)
	{
		LPVOID lpBase = MapViewOfFile(mapFileHandle, FILE_MAP_ALL_ACCESS, 0, 0, 0);
		// 将共享内存数据拷贝出来
		char szBuffer[BUF_SIZE] = { 0 };
		//strcpy(szBuffer, (char*)lpBase);
		memcpy(szBuffer, lpBase, 24);
		printf("%s", szBuffer);

		// 解除文件映射
		UnmapViewOfFile(lpBase);
		// 关闭内存映射文件对象句柄
		CloseHandle(mapFileHandle);
	}
	else
	{
		// 打开共享内存句柄失败
		printf("OpenMapping Error");
	}
	system("pause");
	return 0;
}
```

##### 参考

[https://blog.csdn.net/tojohnonly/article/details/70246965]()

###### Linux 

[https://www.yiibai.com/ipc/shared_memory.html]()