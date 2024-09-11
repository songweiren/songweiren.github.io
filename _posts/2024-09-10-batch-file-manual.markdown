---
layout: post
title:  "bat文件教程及常用实例"
date:   2024-09-11 10:49:52 +0800
categories: bat  
tags: bat
---
#### 教程
##### 1 系统变量
他们的值由系统将其根据事先定义的条件⾃动赋值,也就是这些变量系统已经给他们定义了值,不需要我们来给他赋值,我们只需要调⽤⽽以! 我把他们全部列出来!
``` bash
%ALLUSERSPROFILE% 本地 返回“所有⽤户”配置⽂件的位置。
%APPDATA% 本地 返回默认情况下应⽤程序存储数据的位置。
%CD% 本地 返回当前⽬录字符串。
%CMDCMDLINE% 本地 返回⽤来启动当前的 Cmd.exe 的准确命令⾏。
%CMDEXTVERSION% 系统 返回当前的“命令处理程序扩展”的版本号。
%COMPUTERNAME% 系统 返回计算机的名称。
%COMSPEC% 系统 返回命令⾏解释器可执⾏程序的准确路径。
%DATE% 系统 返回当前⽇期。使⽤与 date /t 命令相同的格式。由 Cmd.exe ⽣成。有关date 命令的详细信息，请参阅 Date。
%ERRORLEVEL% 系统 返回上⼀条命令的错误代码。通常⽤⾮零值表⽰错误。
%HOMEDRIVE% 系统 返回连接到⽤户主⽬录的本地⼯作站驱动器号。基于主⽬录值⽽设置。⽤户主⽬录是在“本地⽤户和组”中指定的。
%HOMEPATH% 系统 返回⽤户主⽬录的完整路径。基于主⽬录值⽽设置。⽤户主⽬录是在“本地⽤户和组”中指定的。
%HOMESHARE% 系统 返回⽤户的共享主⽬录的⽹络路径。基于主⽬录值⽽设置。⽤户主⽬录是在“本地⽤户和组”中指定的。
%LOGONSERVER% 本地 返回验证当前登录会话的域控制器的名称。
%NUMBER_OF_PROCESSORS% 系统 指定安装在计算机上的处理器的数⽬。
%OS% 系统 返回操作系统名称。Windows 2000 显⽰其操作系统为 Windows_NT。
%PATH% 系统 指定可执⾏⽂件的搜索路径。
%PATHEXT% 系统 返回操作系统认为可执⾏的⽂件扩展名的列表。
%PROCESSOR_ARCHITECTURE% 系统 返回处理器的芯⽚体系结构。值：x86 或 IA64 基于Itanium
%PROCESSOR_IDENTFIER% 系统 返回处理器说明。
%PROCESSOR_LEVEL% 系统 返回计算机上安装的处理器的型号。
%PROCESSOR_REVISION% 系统 返回处理器的版本号。
%PROMPT% 本地 返回当前解释程序的命令提⽰符设置。由 Cmd.exe ⽣成。
%RANDOM% 系统 返回 0 到 32767 之间的任意⼗进制数字。由 Cmd.exe ⽣成。
%SYSTEMDRIVE% 系统 返回包含 Windows server operating system 根⽬录（即系统根⽬录）的驱动器。
%SYSTEMROOT% 系统 返回 Windows server operating system 根⽬录的位置。
%TEMP% 和 %TMP% 系统和⽤户 返回对当前登录⽤户可⽤的应⽤程序所使⽤的默认临时⽬录。有些应⽤程序需要 TEMP，⽽其他应⽤程序则需要 TMP。
%TIME% 系统 返回当前时间。使⽤与 time /t 命令相同的格式。由 Cmd.exe ⽣成。有关time 命令的详细信息，请参阅 Time。
%USERDOMAIN% 本地 返回包含⽤户帐户的域的名称。
%USERNAME% 本地 返回当前登录的⽤户的名称。
%USERPROFILE% 本地 返回当前⽤户的配置⽂件的位置。
%WINDIR% 系统 返回操作系统⽬录的位置。

```
这么多系统变量,我们如何知道他的值是什么呢?
在CMD⾥输⼊ echo %WINDIR%这样就能显⽰⼀个变量的值了!
举个实际例⼦,⽐如我们要复制⽂件到当前帐号的启动⽬录⾥就可以这样
copy d:\1.bat "%USERPROFILE%\「开始」菜单\程序\启动"
%USERNAME% 本地 返回当前登录的⽤户的名称。 注意有空格的⽬录要⽤引号引起来
另外还有⼀些系统变量,他们是代表⼀个意思,或者⼀个操作!
他们分别是%0 %1 %2 %3 %4 %5 ......⼀直到%9 还有⼀个%*
``` bash
%0 这个有点特殊,有⼏层意思,先讲%1-%9的意思.
%1 返回批处理的第⼀个参数
%2 返回批处理的第⼆个参数
%3-%9依此推类
反回批处理参数?到底怎么个返回法?
我们看这个例⼦,把下⾯的代码保存为test.BAT然后放到C盘下
```
##### 2 批处理常用内部命令

##### 3 批处理常用特殊符号
1、@ 命令⾏回显屏蔽符
2、% 批处理变量引导符
3、> 重定向符
4、>> 重定向符
5、<、>&、<& 重定向符
6、| 命令管道符
7、^ 转义字符
8、& 组合命令
9、&& 组合命令
10、|| 组合命令
11、"" 字符串界定符
12、, 逗号
13、; 分号
14、() 括号
15、! 感叹号
16、批处理中可能会见到的其它特殊标记符: （略）
CR(0D) 命令⾏结束符
Escape(1B) ANSI转义字符引导符
Space(20) 常⽤的参数界定符
Tab(09) ; = 不常⽤的参数界定符
COPY命令⽂件连接符
? ⽂件通配符
/ 参数开关引导符
: 批处理标签引导符


##### 4 自定义变量
⾃定义变量就是由我们来给他赋予值的变量要使⽤⾃定义变量就得使⽤set命令了
``` bash
@echo off
set var=我是值
echo %var%
pause

```
保存为BAT执⾏,我们会看到CMD⾥返回⼀个 "我是值"
var为变量名,=号右变的是要给变量的值这就是最简单的⼀种设置变量的⽅法了
如果我们想让⽤户⼿⼯输⼊变量的值,⽽不是在代码⾥指定,可以⽤⽤set命令的/p参数
例⼦:
``` bash
@echo off
set /p var=请输⼊变量的值
echo %var%
pause

```
var变量名 =号右边的是提⽰语,不是变量的值变量的值由我们运⾏后⾃⼰⽤键盘输⼊!
##### 5 条件if-else


##### 6 循环(for/while)




#### 常用实例
##### 1 交互界面设计
没啥说的，看看⾼⼿设计的菜单界⾯吧：
``` bash
@echo off
cls
title 终极多功能修复
:menu
cls
color 0A
echo.
echo ==============================
echo 请选择要进⾏的操作，然后按回车
echo ==============================
echo.
echo 1.⽹络修复及上⽹相关设置,修复IE,⾃定义屏蔽⽹站
echo.
echo 2.病毒专杀⼯具，端⼝关闭⼯具,关闭⾃动播放
echo.
echo 3.清除所有多余的⾃启动项⽬，修复系统错误
echo.
echo 4.清理系统垃圾,提⾼启动速度
echo.
echo Q.退出
echo.
echo.
:cho
set choice=
set /p choice= 请选择:
IF NOT "%choice%"=="" SET choice=%choice:~0,1%
if /i "%choice%"=="1" goto ip
if /i "%choice%"=="2" goto setsave
if /i "%choice%"=="3" goto kaiji
if /i "%choice%"=="4" goto clean
if /i "%choice%"=="Q" goto endd
echo 选择⽆效，请重新输⼊
echo.
goto ch
```



> [bat脚本教程v1.0.pdf](https://data-release-yjw.oss-cn-beijing.aliyuncs.com/files/bat/bat%E8%84%9A%E6%9C%AC%E6%95%99%E7%A8%8Bv1.0.pdf) <br>
> [Windows 批处理脚本学习教程](http://docs.30c.org/dosbat/)
