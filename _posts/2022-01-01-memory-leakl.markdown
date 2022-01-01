---
layout: post
title:  "Memory Leak"
date:   2022-01-01 15:49:52 +0800
categories: C++   
tags: C++
---
### 常见内存泄漏场景

计算机常见的内存分配有栈区，堆区，全局变量区，代码区等。 一些局部变量在栈区的话，它的内存申请和释放是由操作系统完成的。而通过new和malloc申请的内存是在堆区，这部分内存需要程序员自己手动释放，操作系统不负责这部分内存的管理。如果我们通过这种方式在堆上申请了内存，用完之后没有手动区释放这部分内存，它一直会被应用程序占用，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。

#####  1. 类似<font color=#0000FF> Vector</font>的数据结构，不断的<font color=#0000FF> push_back()</font>数据进去，但没有去清理内存

```c++
vector<int> data;
//push data to vector
data.push_back(1);
//do sth.else

//clear 
data.clear();//紧清空内容，并不释放内存
vector<int>().swap(data); // 清空内容并释放内存
```

#####  2. 调用<font color=#0000FF> open()/opendir() </font>等打开资源的操作之后，没有调用close关闭资源

```c++
int fd = open("test.txt",O_RDONLY)；
//to do something else 
//...
 close(fd);
```

#####  3. 用<font color=#0000FF> malloc()/new()</font>向操作系统申请内存以后，没有调用<font color=#0000FF> free / delete</font>等释放资源

```c++
char* a = (char*)malloc(sizeof(char));
// to do something else
//...
free(a);//释放内存
```

另外一种情况

```c++
//申请两个
int * p =(int*)malloc(sizeof(int));
int * q =(int*)malloc(sizeof(int));
//
p=q;
//释放内存，有问题吗？
free(p);
free(q);
```

数组的释放

```c++
//数组
Char *p = new char[10]
delete p;//delete[] p
```

##### 4. 智能指针

```c++
unique_ptr<int> p1 (new int(3));

p1.reset();//释放所有权并删除管理对象
p1.release();//返回指针并释放所有权
int a=*p2; //此时访问原对象会出现不确定值

//如果调用release()释放所有权以后，这里需要手动释放空间。若没掉release或者是调用了reset，这里使用free手动释放会出问题  
//free(p2);
cout<<a<<endl;
```



##### 5. 基类的析构函数未定义未虚函数，

由于c++的动态绑定特性，当基类指针指向子类对象时，如果基类的析构函数不是virtual，那么子类的析构函数将不会被调用，子类的资源没有正确是释放，因此造成内存泄露。

##### 6. 缺少拷贝构造函数

在类里存在成员变量是指针时，在进行赋值=运算和按值传参时，必须重载拷贝构造函数，重新实现其指针拷贝的部分

```c++

class Person
{
  public:
    	int age;
    	int* high;
    Person()
    {
        
    }
    Person(Person & pe)
    {
        age=pe.age;
        //浅拷贝
        high = pe.high;
        //深拷贝
        high= new int*(*(pe.high));
    }
    ~Person()
    {
        delete high;
    }
};
```



### Linux上常用内存泄漏的调查方法

##### 1. Valgrind

Valgrind是一个GPL的软件，用于Linux（For x86, amd64 and ppc32）程序的内存调试和代码剖析。你可以在它的环境中运行你的程序来监视内存的使用情况，比如C 语言中的malloc和free或者 C++中的new和 delete。使用Valgrind的工具包，你可以自动的检测许多内存管理和线程的bug，避免花费太多的时间在bug寻找上，使得你的程序更加稳固。

Valgrind工具包包含多个工具，如Memcheck,Cachegrind,Helgrind, Callgrind，Massif。

我们常用的是memcheck工具，要检查下面的程序错误：

- 使用未初始化的内存 (Use of uninitialised memory)
- 使用已经释放了的内存 (Reading/writing memory after it has been free’d)
- 使用超过 malloc分配的内存空间(Reading/writing off the end of malloc’d blocks)
- 对堆栈的非法访问 (Reading/writing inappropriate areas on the stack)
- 申请的空间是否有释放 (Memory leaks – where pointers to malloc’d blocks are lost forever)
- malloc/free/new/delete申请和释放内存的匹配(Mismatched use of malloc/new/new [] vs free/delete/delete [])
- src和dst的重叠(Overlapping src and dst pointers in memcpy() and related functions)

常用的使用方法如下，使用valgrind命令行启动程序

```bash
Valgrind --tool=memcheck --leak-check=full --trace-children=yes --log-file=./myproc.log UIH/bin/SIBTracer
# option 
#--show-leak-kinds=all
```

运行完毕以后会在当前目录生成一个myproc.log的log文件（名字是命令行中指定的，可自行修改）。

在log的结尾部分会有如下记录：

```latex
==30023== LEAK SUMMARY:
==30023==    definitely lost: 65,632 bytes in 2 blocks
==30023==    indirectly lost: 0 bytes in 0 blocks
==30023==      possibly lost: 279,160 bytes in 2,825 blocks
==30023==    still reachable: 219,062 bytes in 2,254 blocks
==30023==         suppressed: 0 bytes in 0 blocks
==30023== 
==30023== ERROR SUMMARY: 953 errors from 953 contexts (suppressed: 0 from 0)
```

其中<font color=red>definitely lost</font>即为存在内存泄漏的地方，如果是0表示不存在内存泄漏，如果不为0即有内存泄漏。可在log中搜索<font color=Red>definitely</font>关键字查看内存泄漏的地方。

```latex
==30023== 32,816 bytes in 1 blocks are definitely lost in loss record 2,516 of 2,518
==30023==    at 0x4C2D06F: malloc (in /usr/local/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==30023==    by 0x7D1B813: __alloc_dir (opendir.c:247)
==30023==    by 0x7D1B902: opendir_tail (opendir.c:145)
==30023==    by 0x120499: getsibuioindex (main.cpp:113)
==30023==    by 0x120499: sibOpen(int*) (main.cpp:139)
==30023==    by 0x11FE77: main (main.cpp:684)
```



##### 2. 观察*RSS*(resident set size)监控数据

即运行***ps aux***指令后的一些资源统计数据，它表示常驻内存的大小，但是由于不同的进程之间会共享内存，所以把所有进程rss进行累加的方法会重复计算共享内存，得到的结果是偏大的。

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-M5YF2x2U-1639376133577)(D:\work\memoryleak\image\psaux.png)]



### Linux下常用的系统资源监控的命令和文件

##### 1. cat /proc/meminfo

以下内容取自开发机

```bash
MemTotal:       32933528 kB  # 所有可用RAM大小（即物理内存减去一些预留位和内核的二进制代码大小）（HighTotal + LowTotal）,系统从加电开始到引导完成，BIOS等要保留一些内存，内核要保留一些内存，最后剩下可供系统支配的内存就是MemTotal。这个值在系统运行期间一般是固定不变的。
MemFree:          581020 kB  # LowFree与HighFree的总和，被系统留着未使用的内存,MemFree是说的系统层面
MemAvailable:   15526472 kB  # 应用程序可用内存数。系统中有些内存虽然已被使用但是可以回收的，比如cache/buffer、slab都有一部分可以回收，所以MemFree不能代表全部可用的内存，这部分可回收的内存加上MemFree才是系统可用的内存，即：MemAvailable≈MemFree+Buffers+Cached，它是内核使用特定的算法计算出来的，是一个估计,MemAvailable是说的应用程序层面
Buffers:         1367140 kB  # 给文件的缓冲区
Cached:         13221132 kB  # 高速缓冲存储器（等于 diskcache minus SwapCache ）
SwapCached:       235348 kB  # 被高速缓冲存储器（cache memory）用的交换空间的大小，已经被交换出来的内存，但仍然被存放在swapfile中。用来在需要的时候很快的被替换而不需要再次打开I/O端口
Active:         18934800 kB  # 在活跃使用中的缓冲或高速缓冲存储器页面文件的大小，除非非常必要否则不会被移作他用. (Active(anon) + Active(file))
Inactive:       11658248 kB  # 活跃的与文件无关的内存（比如进程的堆栈，用malloc申请的内存）(anonymous pages),anonymous pages在发生换页时，是对交换区进行读/写操作
Active(anon):   13757760 kB
Inactive(anon):  2799728 kB
Active(file):    5177040 kB
Inactive(file):  8858520 kB
Unevictable:          32 kB
Mlocked:              32 kB
SwapTotal:       8385532 kB  # 交换空间总大小
SwapFree:        1996604 kB  # 未被使用交换空间的大小
Dirty:                36 kB  # 等待被写回到磁盘的内存大小
Writeback:             0 kB  # 正在被写回到磁盘的内存大
AnonPages:      15770336 kB  # 未映射页的内存大小
Mapped:          1272920 kB  # 设备和文件等映射的大小
Shmem:            552696 kB
Slab:            1461620 kB  # 内核数据结构缓存的大小，可以减少申请和释放内存带来的消耗
SReclaimable:    1376692 kB  # 可收回Slab的大小
SUnreclaim:        84928 kB  # 不可收回Slab的大小（SUnreclaim+SReclaimable＝Slab
KernelStack:       19888 kB  # 常驻内存,每一个用户线程都会分配一个kernel stack(内核栈)
PageTables:        93764 kB  # 管理内存分页页面的索引表的大小
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:    24852296 kB
Committed_AS:   28715236 kB
VmallocTotal:   34359738367 kB  # 可以vmalloc虚拟内存大小
VmallocUsed:           0 kB     # vmalloc已使用的虚拟内存大小
VmallocChunk:          0 kB     # 最大的连续未被使用的vmalloc区域
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      462656 kB
DirectMap2M:    28897280 kB
DirectMap1G:     6291456 kB
```

参考: [cat /proc/meminfo 各字段详解-csdn](https://blog.csdn.net/JustDoIt_201603/article/details/106629059?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.nonecase)



##### 2. ps 

ps是process status的缩写，用于显示当前进程的状态。使用该命令可以确定有哪些进程 正在运行 和 运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多的资源等。

**使用方法：**

```bash
ps [options] [--help]
```

**参数 **

  - ps 的参数非常多, 在此仅列出几个常用的参数并大略介绍含义

  - -A 列出所有的进程

  - -e 显示所有进程

  - -d 显示除控制进程外的所有进程

  - -f 显示完整格式的输出

  - -L 显示进程中的线程

  - -w 显示加宽可以显示较多的资讯

  - -au 显示较详细的资讯

  - -aux 显示所有包含其他使用者的行程

  - au(x) 输出格式 :

      ```bash
      USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
      ```

    - USER: 进程拥有者
    - PID: pid
    - %CPU: 占用的 CPU 使用率
    - %MEM: 占用的内存使用率
    - VSZ: 占用的虚拟内存大小
    - RSS: 进程占用的内存体大小
    - TTY: 终端的次要装置号码 (minor device number of tty)
    - STAT: 该行程的状态:
      - D: 无法中断的休眠状态 (通常 IO 的进程)
      - R: 正在执行中
      - S: 静止状态
      - T: 暂停执行
      - Z: 不存在但暂时无法消除
      - W: 没有足够的记忆体分页可分配
      - <: 高优先序的行程
      - N: 低优先序的行程
    - START: 行程开始时间
    - TIME: 执行的时间
    - COMMAND:所执行的指令

**示例：**

查找指定进程格式：

```bash
ps -ef | grep 进程关键字 #ps -ef 为显示所有进程
```

显示进程信息：

```bash
ps -A 
```

显示指定用户信息

```bash
ps -u root # 显示root进程用户信息
```



##### 3. top

top用来监控linux的系统状况，是常用的性能分析工具，能够实时显示系统中各个进程的资源占用情况。top是一个动态显示过程,即可以通过用户按键来不断刷新当前状态。如果在前台执行该命令,它将独占前台,直到用户终止该程序为止.比较准确的说,top命令提供了实时的对系统处理器的状态监视.它将显示系统中CPU最“敏感”的任务列表.该命令可以按CPU使用。内存使用和执行时间对任务进行排序；而且该命令的很多特性都可以通过交互式命令或者在个人定制文件中进行设定。

**使用方法**：

```bash
top [-] [d delay] [q] [c] [S] [s] [i] [n] [b]
```

**参数说明**：

- d : 改变显示的更新速度，或是在交谈式指令列( interactive command)按 s
- q : 没有任何延迟的显示速度，如果使用者是有 superuser 的权限，则 top 将会以最高的优先序执行
- c : 切换显示模式，共有两种模式，一是只显示执行档的名称，另一种是显示完整的路径与名称
- S : 累积模式，会将己完成或消失的子进程 ( dead child process ) 的 CPU time 累积起来
- s : 安全模式，将交谈式指令取消, 避免潜在的危机
- i : 不显示任何闲置 (idle) 或无用 (zombie) 的进程
- n : 更新的次数，完成后将会退出 top
- b : 批次档模式，搭配 "n" 参数一起使用，可以用来将 top 的结果输出到档案内

**示例**：

设置信息更新次数

```bash
top -n 2 # 更新两次后停止
```

设置信息更新时间

```bash
top -d 3 # 设置更新周期为3s
```

显示指定的进程信息

```bash
top -p 139 # 显示进程号为139的进程信息、CPU、内存占用率等信息
```

**top前5行统计信息**

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-biKXpbAe-1639376133579)(D:\work\memoryleak\image\top)]

- 第一行：<b><font color=LawnGreen>top - 11:32:54 ip 39days, 2:05, 3 users, load average: 0.54, 0.18, 0.19</font></b>
  - 11:32:54                                   --> 表示当前时间
  - up 30days， 2：05                 -->表示运行时间
  - 3 users                                       --> 当前登录用户数
  - load average: 0.54,0.18, 0.09  --> 系统负载，即任务队列的平均长度。三个值分别为1分钟、5分钟、15分钟到现在的平均值

- 第二行：<b><font color=LawnGreen>Tasks: 261 total, 2 running, 257 sleeping ,0 stopped, 2 zombie</font></b>

- 第三行：<b><font color=LawnGreen>%Cpu(s): 19.3 us, 2.5 sy, 0.0ni, 78.0 id, 0.0 wa, 0.0 hi, 0.2 si, 0.0s st </font></b>

  这两行是进程和CPU的信息

  - 261 total       --> 总进程数
  -  2 running     --> 正在运行的进程数
  - 257 sleeping --> 睡眠的进程数
  - 0 stopped      --> 停止的进程数
  - 0 zombie       --> 僵尸进程数
  -   .....

- 第四行：<b><font color=LawnGreen>KiB Mem: 32933528 total, 2145392 free, 15491800 used, 15296336 buff/cache</font></b>

- 第五行：<b><font color=LawnGreen>KiB Swap: 8385532 total, 1997240 free, 6388292 used, 16419048 avail Mem</font></b>

  这两行为内存信息

  - KiB Mem: 32933528        --> 物理内存总量
  - 15491800 used                 --> 使用的物理内存总量
  - 2145392 free                     --> 空闲内存总量
  - 15296336 buff/cache       --> 用作内核缓冲的内存量
  - KiB Swap: 8385532 total  --> 交换区总量
  - 6388292 used                    --> 使用的交换区总量
  - 1997240 free                     --> 空闲交换区总量
  - 16419048 avail Mem        --> 代表可用于进程下一次分配的物理内存数量
