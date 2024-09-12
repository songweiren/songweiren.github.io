---
layout: post
title:  "bat文件教程及常用实例"
date:   2024-09-11 10:49:52 +0800
categories: bat  
tags: bat
---
#### 教程
##### 1 系统变量 <br>
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
- REM and :: <br>
REM为注释命令，⼀般⽤来给程序加上注解，该命令后的内容不被执⾏，但能回显。其次, :: 也可以起到rem 的注释作⽤, ⽽且更简洁有效; 但有两点需要注意：
第⼀, 任何以冒号:开头的字符⾏, 在批处理中都被视作标号, ⽽直接忽略其后的所有内容。有效标号：冒号后紧跟⼀个以字母数字开头的字符串，goto语句可以识别。⽆效标号：冒号后紧跟⼀个⾮字母数字的⼀个特殊符号，goto⽆法识别的标号，可以起到注释作⽤，所以 :: 常被⽤
作注释符号，其实 :+ 也可起注释作⽤。
第 ⼆, 与rem 不同的是, ::后的字符⾏在执⾏时不会回显, ⽆论是否⽤echo on打开命令⾏回显状态, 因为命令解释器不认为他是⼀个有效的命令⾏, 就此点来看, rem 在某些场合下将⽐ :: 更为适⽤; 另外, rem 可以⽤于config.sys ⽂件中。
⾏内注释格式：%注释内容% （不常⽤，慎⽤）
- ECHO and @ <br>
  @字符放在命令前将关闭该命令回显，⽆论此时echo是否为打开状态。
  1. 打开回显或关闭回显功能
  格式:echo [{ on|off }]
  如果想关闭“ECHO OFF”命令⾏⾃⾝的显⽰，则需要在该命令⾏前加上“@”。
  2. 显⽰当前ECHO设置状态
  格式:echo
  3. 输出提⽰信息
  格式：ECHO 信息内容
  上述是ECHO命令常见的三种⽤法，也是⼤家熟悉和会⽤的，但作为DOS命令淘⾦者你还应该知道下⾯的技巧：
  4. 关闭DOS命令提⽰符
  在DOS提⽰符状态下键⼊ECHO OFF，能够关闭DOS提⽰符的显⽰使屏幕只留下光标，直⾄键⼊ECHO ON，提⽰符才会重新出现。
  5. 输出空⾏，即相当于输⼊⼀个回车
  格式：ECHO．
  值得注意的是命令⾏中的“．”要紧跟在ECHO后⾯中间不能有空格，否则“．”将被当作提⽰信息输出到屏幕。另外“．”可以⽤，：；”／ []＋等 
  任⼀符号替代。
  命令ECHO．输出的回车，经DOS管道转向可以作为其它命令的输⼊，⽐如echo.|time即相当于在TIME命令执⾏后给出⼀个回车。所以执⾏时系会在显⽰当前时间后，⾃动返回到DOS提⽰符状态
  6. 答复命令中的提问
  格式：ECHO 答复语|命令⽂件名
  上述格式可以⽤于简化⼀些需要⼈机对话的命令（如：CHKDSK／F；FORMAT Drive:；del .）的操作，它是通过DOS管道命令把ECHO命令输出的预置答复语作为⼈机对话命令的输⼊。下⾯的例⼦就相当于在调⽤的命令出现⼈机对话时输⼊“Y”回车：
  C:>ECHO Y|CHKDSK/F
  C:>ECHO Y|DEL A :.
  7. 建⽴新⽂件或增加⽂件内容
  格式：ECHO ⽂件内容>⽂件名
  ECHO ⽂件内容>>⽂件名
  例如：
``` bash
C:>ECHO @ECHO OFF>AUTOEXEC.BAT建⽴⾃动批处理⽂件
C:>ECHO C:\CPAV\BOOTSAFE>>AUTOEXEC.BAT向⾃动批处理⽂件中追加内容
C:>TYPE AUTOEXEC.BAT显⽰该⾃动批处理⽂件
@ECHO OFF
C:\CPAV\BOOTSAFE
```

  8. 向打印机输出打印内容或打印控制码
  格式：ECHO 打印机控制码>;PRN
``` bash
  ECHO 打印内容>;PRN
``` 
  下⾯的例⼦是向M－1724打印机输⼊打印控制码。＜Alt＞156是按住Alt键在⼩键盘键⼊156，类似情况依此类
``` bash
C:>ECHO +156+42+116>;PRN（输⼊下划线命令FS＊t）
C:>ECHO \[email=+155@]+155@>;PRN\[/email]（输⼊初始化命令ESC@）
C:>ECHO.>;PRN（换⾏）
```

  9. 使喇叭鸣响
``` bash
  C:>ECHO ^G
``` 
  “G”是在dos窗⼝中⽤Ctrl＋G或Alt＋007输⼊，输⼊多个G可以产⽣多声鸣响。使⽤⽅法是直接将其加⼊批处理⽂件中或做成批处理⽂件调⽤,这⾥的“^G”属于特殊符号的使⽤，请看本⽂后⾯的章节

- errorlevel <br>
程序返回码
每个命令运⾏结束，可以⽤这个命令⾏格式查看返回码,⽤于判断刚才的命令是否执⾏成功
默认值为0，⼀般命令执⾏出错会设 errorlevel 为1
``` bash
echo %errorlevel%
``` 

- title <br>
设置cmd窗⼝的标题
title 新标题 #可以看到cmd窗⼝的标题栏变了
- COLOR <br>
设置默认的控制台前景和背景颜⾊。
COLOR [attr]
attr 指定控制台输出的颜⾊属性
颜⾊属性由两个⼗六进制数字指定 -- 第⼀个为背景，第⼆个则为
前景。每个数字可以为以下任何值之一:
```
0 = 黑色 8 = 灰色
1 = 蓝色 9 = 淡蓝色
2 = 绿色 A = 淡绿色
3 = 湖蓝色 B = 淡浅绿色
4 = 红色 C = 淡红色
5 = 紫色 D = 淡紫色
6 = 黄色 E = 淡黄色
7 = 白色 F = 亮白色
``` 
如果没有给定任何参数，该命令会将颜⾊还原到 CMD.EXE 启动时
的颜⾊。这个值来⾃当前控制台窗⼝、/T 开关或
DefaultColor 注册表值。
如果⽤相同的前景和背景颜⾊来执⾏ COLOR 命令，COLOR 命令
会将 ERRORLEVEL 设置为 1。
例如: "COLOR fc" 在亮⽩⾊上产⽣亮红⾊
- find <br>
在⽂件中搜索字符串。
``` shell
FIND [/V] [/C] [/N] [/I] [/OFF[LINE]] "string" [[drive:][path]f ilename[ ...]]
/V 显⽰所有未包含指定字符串的⾏。
/C 仅显⽰包含字符串的⾏数。
/N 显⽰⾏号。
/I 搜索字符串时忽略⼤⼩写。
/OFF[LINE] 不要跳过具有脱机属性集的⽂件。
"string" 指定要搜索的⽂字串，
[drive:][path]f ilename
指定要搜索的⽂件。
如果没有指定路径，FIND 将搜索键⼊的或者由另⼀命令产⽣的⽂字。
Find常和type命令结合使⽤
```
Type [drive:][path]filename | find "string" [>tmpfile] #挑选包含string的⾏
Type [drive:][path]filename | find /v "string" #剔除⽂件中包含string的⾏
Type [drive:][path]filename | find /c #显⽰⽂件⾏数
- start 命令<br>
批处理中调⽤外部程序的命令（该外部程序在新窗⼝中运⾏，批处理程序继续往下执⾏，不理会外部程序的运⾏状
况），如果直接运⾏外部程序则必须等外部程序完成后才继续执⾏剩下的指令
例：start explorer d:\ 调⽤图形界⾯打开D盘
- CALL <br>
CALL命令可以在批处理执⾏过程中调⽤另⼀个批处理，当另⼀个批处理执⾏完后，再继续执⾏原来的批处理
CALL command
调⽤⼀条批处理命令，和直接执⾏命令效果⼀样，特殊情况下很有⽤，⽐如变量的多级嵌套，见教程后⾯。在批处
理编程中，可以根据⼀定条件⽣成命令字符串，⽤call可以执⾏该字符串，见例⼦。
CALL [drive:][path]filename [batch-parameters]
调⽤的其它批处理程序。filename 参数必须具有 .bat 或 .cmd 扩展名。
CALL :label arguments
调⽤本⽂件内命令段，相当于⼦程序。被调⽤的命令段以标签:label开头
以命令goto :eof结尾。
另外，批脚本⽂本参数参照(%0、%1、等等)已如下改变:
批脚本⾥的 %* 指出所有的参数(如 %1 %2 %3 %4 %5 ...)
批参数(%n)的替代已被增强。您可以使⽤以下语法:（看不明⽩的直接运⾏后⾯的例⼦）
```
%~1 - 删除引号(")，扩充 %1
%~f 1 - 将 %1 扩充到⼀个完全合格的路径名
%~d1 - 仅将 %1 扩充到⼀个驱动器号
%~p1 - 仅将 %1 扩充到⼀个路径
%~n1 - 仅将 %1 扩充到⼀个⽂件名
%~x1 - 仅将 %1 扩充到⼀个⽂件扩展名
%~s1 - 扩充的路径指含有短名
%~a1 - 将 %1 扩充到⽂件属性
%~t1 - 将 %1 扩充到⽂件的⽇期/时间
%~z1 - 将 %1 扩充到⽂件的⼤⼩
%~PATH:1−查找列在PATH环境变量的⽬录，并将PATH:1 - 在列在 PATH 环境变量中的⽬录⾥查找 %1，
并扩展到找到的第⼀个⽂件的驱动器号和路径。
%~f tza1 - 将 %1 扩展到类似 DIR 的输出⾏。
在上⾯的例⼦中，%1 和 PATH 可以被其他有效数值替换。
%~ 语法被⼀个有效参数号码终⽌。%~ 修定符不能跟 %*使⽤
```
注意：参数扩充时不理会参数所代表的⽂件是否真实存在，均以当前⽬录进⾏扩展
要理解上⾯的知识，下⾯的例⼦很关键。
``` bash
@echo off
Echo 产⽣⼀个临时⽂件 > tmp.txt
Rem 下⾏先保存当前⽬录，再将c:\windows设为当前⽬录
pushd c:\windows
Call :sub tmp.txt
Rem 下⾏恢复前次的当前⽬录
Popd
Call :sub tmp.txt
pause
Del tmp.txt
exit
:sub
Echo 删除引号： %~1
Echo 扩充到路径： %~f1
Echo 扩充到⼀个驱动器号： %~d1
Echo 扩充到⼀个路径： %~p1
Echo 扩充到⼀个⽂件名： %~n1
Echo 扩充到⼀个⽂件扩展名： %~x1
Echo 扩充的路径指含有短名： %~s1
Echo 扩充到⽂件属性： %~a1
Echo 扩充到⽂件的⽇期/时间： %~t1
Echo 扩充到⽂件的⼤⼩： %~z1
Echo 扩展到驱动器号和路径：%~dp1
Echo 扩展到⽂件名和扩展名：%~nx1
Echo 扩展到类似 DIR 的输出⾏：%~ftza1
Echo.
Goto :eof
```
例子
``` bash
set aa=123456
set cmdstr=echo %aa%
call %cmdstr%
pause
```

- shift <br>
更改批处理⽂件中可替换参数的位置。
SHIFT[/n]
如果命令扩展名被启⽤，SHIFT 命令⽀持/n 命令⾏开关；该:命令⾏开关告诉
命令从第 n 个参数开始移位；n 介于零和⼋之间。
例如:
SHIFT /2
会将 %3 移位到 %2，将 %4 移位到 %3，等等；并且不影响 %0 和 %1。
- ATTRIB 显示或更改文件件属性 <br>
ATTRIB [+R|-R] [+A|-A] [+S|-S] [+H|-H] [[drive:] [path] filename] [/S [/D]]
```
+ 设置属性。
- 清除属性。
R 只读⽂件属性。
A 存档⽂件属性。
S 系统⽂件属性。
H 隐藏⽂件属性。
[drive:][path][f ilename]
指定要处理的⽂件属性。
/S 处理当前⽂件夹及其⼦⽂件夹中的匹配⽂件。
/D 也处理⽂件夹。
```
例
``` bash
md autorun
attrib +a +s +h autorun
``` 
- pend
- pend
- pend
- 

##### 3 批处理常用特殊符号
 - @ 命令⾏回显屏蔽符 <br>
  这个字符在批处理中的意思是关闭当前⾏的回显。我们从前⼏课知道
ECHO OFF可以关闭掉整个批处理命令的回显，但不能关掉ECHO OFF这个命令，现在我们在ECHO OFF这个命
令前加个@，就可以达到所有命令均不回显的要求
 - % 批处理变量引导符 <br>
  这个百分号严格来说是算不上命令的，它只是批处理中的参数⽽已（多个%⼀起使⽤的情况除外，以后还将详细介
绍）。
引⽤变量⽤%var%，调⽤程序外部参数⽤%1⾄%9等等
 - \> 重定向符 <br>
输出重定向命令
DOS的标准输⼊输出通常是在标准设备键盘和显⽰器上进⾏的，利⽤重定向,可以⽅便地将输⼊输出改向磁盘⽂件或
其它设备。其中:
1.大于号“>”将命令发送到⽂件或设备，例如打印机>prn。使⽤⼤于号“>”时，有些命令输出(例如错误消息)不能重定
向。
2.双大于号“>>”将命令输出添加到⽂件结尾⽽不删除⽂件中已有的信息。
3.小于号“<”从⽂件⽽不是键盘上获取命令所需的输⼊。
4.>&符号将输出从⼀个默认I/O流(stdout,stdin,stderr)重新定向到另⼀个默认I/O流。
例如，command >output_file 2>&1将处理command过程中的所有错误信息从屏幕重定向到标准⽂件输出中。标
准输出的数值如下所⽰：
命令重定向的标准句柄
 - \>> 重定向符 <br>
  输出重定向命令
这个符号的作⽤和>有点类似，但他们的区别是>>是传递并在⽂件的末尾追加，⽽>是覆盖
⽤法同上
同样拿1.txt做例
 -  <、>&、<& 重定向符
 - | 命令管道符<br>
格式：第⼀条命令 | 第⼆条命令 [| 第三条命令...]
将第⼀条命令的结果作为第⼆条命令的参数来使⽤，记得在unix中这种⽅式很常见。

``` bash
dir c:\|find "txt"
echo y|format a: /s /q /v:system
```
 - ^ 转义字符 <br>
 - & 组合命令 <br>
语法：第⼀条命令 & 第⼆条命令 [& 第三条命令...]
&、&&、||为组合命令，顾名思义，就是可以把多个命令组合起来当⼀个命令来执⾏。这在批处理脚本⾥是允许的，
⽽且⽤的⾮常⼴泛。因为批处理认⾏不认命令数⽬。
这个符号允许在⼀⾏中使⽤2个以上不同的命令，当第⼀个命令执⾏失败了，也不影响后边的命令执⾏。
这⾥&两边的命令是顺序执⾏的，从前往后执⾏。

⽐如：
``` bash
dir z:\ & dir y:\ & dir c:
```
以上命令会连续显⽰z,y,c盘的内容，不理会该盘是否存在
 - && 组合命令 <br>
语法：第⼀条命令 && 第⼆条命令 [&& 第三条命令...]
⽤这种⽅法可以同时执⾏多条命令，当碰到执⾏出错的命令后将不执⾏后⾯的命令，如果⼀直没有出错则⼀直执⾏
完所有命令
这个命令和上边的类似，但区别是，第⼀个命令失败时，后边的命令也不会执⾏
``` bash
dir z:\ && dir y:\ && dir c:
``` 
- || 组合命令 <br>
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
 - "" 字符串界定符 <br>
双引号允许在字符串中包含空格，进⼊⼀个特殊⽬录可以⽤如下⽅法
``` bash
cd "program files"
cd progra~1
cd pro*
``` 
以上三种⽅法都可以进⼊program files这个⽬录
 - , 逗号 <br>
 - ; 分号 <br>
 - () 括号 <br>
 - ! 感叹号 <br>
 没啥说的，在变量延迟问题中，⽤来表⽰变量，即%var%应该表⽰为!var!
 - 批处理中可能会见到的其它特殊标记符: （略）
 - CR(0D) <br> 命令⾏结束符
 - Escape(1B) <br> ANSI转义字符引导符
 - Space(20) <br>常⽤的参数界定符
 - Tab(09) ; =  <br>不常用的参数界定符
 - COPY命令⽂件连接符 <br>
 - ? 条件通配符 <br>
 - / 参数开关引导符 <br>
 - : 批处理标签引导符 <br>


##### 4 自定义变量
⾃定义变量就是由我们来给他赋予值的变量要使⽤⾃定义变量就得使⽤set命令了
``` bash
@echo off
set var=我是值
echo %var%
pause

```
保存为BAT执行,我们会看到CMD⾥返回一个 "我是值"
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
没啥说的，看看高价设计的菜单界面吧：
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
