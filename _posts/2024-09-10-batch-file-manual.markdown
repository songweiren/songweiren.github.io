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
 - @ 命令⾏回显屏蔽符
  这个字符在批处理中的意思是关闭当前⾏的回显。我们从前⼏课知道
ECHO OFF可以关闭掉整个批处理命令的回显，但不能关掉ECHO OFF这个命令，现在我们在ECHO OFF这个命
令前加个@，就可以达到所有命令均不回显的要求
 - % 批处理变量引导符
  这个百分号严格来说是算不上命令的，它只是批处理中的参数⽽已（多个%⼀起使⽤的情况除外，以后还将详细介
绍）。
引⽤变量⽤%var%，调⽤程序外部参数⽤%1⾄%9等等
 - ^ > 重定向符
输出重定向命令
DOS的标准输⼊输出通常是在标准设备键盘和显⽰器上进⾏的，利⽤重定向,可以⽅便地将输⼊输出改向磁盘⽂件或
其它设备。其中:
1.⼤于号“>”将命令发送到⽂件或设备，例如打印机>prn。使⽤⼤于号“>”时，有些命令输出(例如错误消息)不能重定
向。
2.双⼤于号“>>”将命令输出添加到⽂件结尾⽽不删除⽂件中已有的信息。
3.⼩于号“<”从⽂件⽽不是键盘上获取命令所需的输⼊。
4.>&符号将输出从⼀个默认I/O流(stdout,stdin,stderr)重新定向到另⼀个默认I/O流。
例如，command >output_file 2>&1将处理command过程中的所有错误信息从屏幕重定向到标准⽂件输出中。标
准输出的数值如下所⽰：
命令重定向的标准句柄
 - ^ >> 重定向符
  输出重定向命令
这个符号的作⽤和>有点类似，但他们的区别是>>是传递并在⽂件的末尾追加，⽽>是覆盖
⽤法同上
同样拿1.txt做例⼦
 - ^ <、>&、<& 重定向符
 - | 命令管道符
格式：第⼀条命令 | 第⼆条命令 [| 第三条命令...]
将第⼀条命令的结果作为第⼆条命令的参数来使⽤，记得在unix中这种⽅式很常见。
``` bash
dir c:\|find "txt"
echo y|format a: /s /q /v:system

```
 - ^ 转义字符
 - & 组合命令
语法：第⼀条命令 & 第⼆条命令 [& 第三条命令...]
&、&&、||为组合命令，顾名思义，就是可以把多个命令组合起来当⼀个命令来执⾏。这在批处理脚本⾥是允许的，
⽽且⽤的⾮常⼴泛。因为批处理认⾏不认命令数⽬。
这个符号允许在⼀⾏中使⽤2个以上不同的命令，当第⼀个命令执⾏失败了，也不影响后边的命令执⾏。
这⾥&两边的命令是顺序执⾏的，从前往后执⾏。
⽐如：
dir z:\ & dir y:\ & dir c:
以上命令会连续显⽰z,y,c盘的内容，不理会该盘是否存在
 - && 组合命令
语法：第⼀条命令 && 第⼆条命令 [&& 第三条命令...]
⽤这种⽅法可以同时执⾏多条命令，当碰到执⾏出错的命令后将不执⾏后⾯的命令，如果⼀直没有出错则⼀直执⾏
完所有命令
这个命令和上边的类似，但区别是，第⼀个命令失败时，后边的命令也不会执⾏
dir z:\ && dir y:\ && dir c:
 - || 组合命令
语法：第⼀条命令 || 第⼆条命令 [|| 第三条命令...]
⽤这种⽅法可以同时执⾏多条命令，当⼀条命令失败后才执⾏第⼆条命令，当碰到执⾏正确的命令后将不执
⾏后⾯的命令，如果没有出现正确的命令则⼀直执⾏完所有命令；
提⽰：组合命令和重定向命令⼀起使⽤必须注意优先级
管道命令的优先级⾼于重定向命令，重定向命令的优先级⾼于组合命令
问题：把C盘和D盘的⽂件和⽂件夹列出到a.txt⽂件中。看例：
``` bash
dir c:\ && dir d:\ > a.txt
```
这 样执⾏后a.txt⾥只有D盘的信息！为什么？因为组合命令的优先级没有重定向命令的优先级⾼！所以这句在执⾏
时将本⾏分成这两部分：dir c:\和dir d:\ > a.txt，⽽并不是如你想的这两部分：dir c:\ && dir d:\和> a.txt。要使⽤
组合命令&&达到题⽬的要求，必须得这么写：
``` bash
dir c:\ > a.txt && dir d:\ >> a.txt
```
 - "" 字符串界定符
双引号允许在字符串中包含空格，进⼊⼀个特殊⽬录可以⽤如下⽅法
cd "program files"
cd progra~1
cd pro*
以上三种⽅法都可以进⼊program files这个⽬录
 - , 逗号
 - ; 分号
 - () 括号
 - ! 感叹号
 - 批处理中可能会见到的其它特殊标记符: （略）
 - CR(0D) 命令⾏结束符
 - Escape(1B) ANSI转义字符引导符
 - Space(20) 常⽤的参数界定符
 - Tab(09) ; = 不常⽤的参数界定符
 - COPY命令⽂件连接符
 - ? ⽂件通配符
 - / 参数开关引导符
 - : 批处理标签引导符


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
##### 2 利⽤for命令延时
``` bash
@echo off
echo 延时前：%time%
for /l %%i in (1,1,5000) do echo %%i>nul
echo 延时后：%time%
pause
```
###### 3 批处理任意时间延迟，+-10ms，误差>50ms
``` bash
@echo off
set /p delay=请输⼊需延迟的毫秒数：
set TotalTime=0
set NowTime=%time%
::读取起始时间，时间格式为：13:01:05.95
echo 程序开始时间：%NowTime%
:delay_continue
set /a minute1=1%NowTime:~3,2%-100
::读取起始时间的分钟数
set /a second1=1%NowTime:~-5,2%%NowTime:~-2%0-100000
::将起始时间的秒数转为毫秒
set NowTime=%time%
set /a minute2=1%NowTime:~3,2%-100
:: 读取现在时间的分钟数
set /a second2=1%NowTime:~-5,2%%NowTime:~-2%0-100000
::将现在时间的秒数转为毫秒
set /a TotalTime+=(%minute2%-%minute1%+60)%%60*60000+%second2%-%second1%
if %TotalTime% lss %delay% goto delay_continue
echo 程序结束时间：%time%
echo 设定延迟时间：%delay%毫秒
echo 实际延迟时间：%TotalTime%毫秒
pause
```
实现原理：⾸先设定要延迟的毫秒数，然后⽤循环累加时间，直到累加时间⼤于等于延迟时间。
误差：windows系统时间只能精确到10毫秒，所以理论上有可能存在10毫秒误差。
经测试，当延迟时间⼤于500毫秒时，上⾯的延迟程序⼀般不存在误差。当延迟时间⼩于500毫秒时，可能有⼏⼗毫
秒误差，为什么？因为延迟程序本⾝也是有运⾏时间的，同时系统时间只能精确到10毫秒。
为了⽅便引⽤，可将上⾯的例⼦改为⼦程序调⽤形式：
``` bash
@echo off
echo 程序开始时间：%Time%
call :delay 10
echo 实际延迟时间：%totaltime%毫秒
echo 程序结束时间：%time%
pause
exit
::-----------以下为延时⼦程序--------------------
:delay
@echo off
if "%1"=="" goto :eof
set DelayTime=%1
set TotalTime=0
set NowTime=%time%
::读取起始时间，时间格式为：13:01:05.95
:delay_continue
set /a minute1=1%NowTime:~3,2%-100
set /a second1=1%NowTime:~-5,2%%NowTime:~-2%0-100000
set NowTime=%time%
set /a minute2=1%NowTime:~3,2%-100
set /a second2=1%NowTime:~-5,2%%NowTime:~-2%0-100000
set /a TotalTime+=(%minute2%-%minute1%+60)%%60*60000+%second2%-%second1%
if %TotalTime% lss %DelayTime% goto delay_continue
goto :eof
```
##### 4 模拟进度条
``` bash
@echo off
mode con cols=113 lines=15 &color 9f
cls
echo.
echo 程序正在初始化. . .
echo.
echo ┌──────────────────────────────────────┐
set/p= ■<nul
for /L %%i in (1 1 38) do set /p a=■<nul&ping /n 1 127.0.0.1>nul
echo 100%%
echo └──────────────────────────────────────┘
pause

```
解释：
解说：“set /p a=■< nul”的意思是：只显⽰提⽰信息“■”且不换⾏，也不需⼿⼯输⼊任何信息，这样可以使每个“■”在同⼀⾏
逐个输出。“ping /n 0 127.1>nul”是输出每个“■”的时间间隔，ping /n 0表⽰不执⾏这个命令，所以会⽐ping出去的时间更
短，也就是即每隔多少时间最短输出⼀个“■”。当然你也可以改为1或2或3等使时间延长
优化：
``` bash
echo.
echo ┌──────────────────────────────────────┐
ping 127.0.0.1 >nul /n 1 & set /p=<nul
for /L %%i in (1 1 39) do set /p a=■<nul & ping /n 1 127.0.0.1>nul
echo 100%%
echo └──────────────────────────────────────┘
pause

```

> [bat脚本教程v1.0.pdf](https://data-release-yjw.oss-cn-beijing.aliyuncs.com/files/bat/bat%E8%84%9A%E6%9C%AC%E6%95%99%E7%A8%8Bv1.0.pdf) <br>
> [Windows 批处理脚本学习教程](http://docs.30c.org/dosbat/)
