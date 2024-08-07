---
layout: post
title:  "Windows批处理脚本(一)"
date:   2019-05-15 22:49:52 +0800
categories: Windows
tags: batch 
---
##### 基本命令
| cmd |  |
|--|--|
| dir 　|　　列文件名|
|cd　　|　　改变当前目录
|ren 　|　　改变文件名
|copy　|　　拷贝文件
|del 　|　　删除文件
|md　|　　　建立子目录
|rd　|　　　删除目录
|deltree|　 删除目录树
|format　|　格式化磁盘
|edit　　|　文本编辑
|type　|　　显示文件内容
|~~mem~~  　　|　~~查看内存状况~~ 
|help　　|　显示帮助提示
|cls 　|　　清屏
|move　|　　移动文件，改目录名
|more　|　　分屏显示
|xcopy 　|　拷贝目录和文件

##### 显示和打印
 - echo
 -- 打印显示
 -- echo on 打开命令的回显
 -- echo off 关闭命令回显
 - @
 如果在某一条命令最前面加上 @ ，那么这一行命令就不会显示出来
 - \>
 表示将输出结果打印到某处，会覆盖之前的
 - \>>
  表示将输出结果打印到某处，不会覆盖之前的
 - title
  后面跟字符串可以改变当前命令提示符的标题名称
 - rem
 可以当作注释命令，和::有异曲同工之妙。
 - pause
 
##### 赋值、调用和参数
###### 赋值
 - 给变量赋值一个字符串
 set var=Hello world，便是赋值操作。
 set var可以查看变量得值
  set v 可以查看所有以字母 v 开头变量的值。直接输入 set 可以查看所有变量的值
 变量两侧加上百分符号 % 用来表示该变量的值(内容)
 - 给变量赋值一个数值型得值
 在 set 后面加上 /a 的参数可以给变量赋予一个数值型的值，例如 set /a var=48 表示将数字48赋给变量var。该数值型的变量是一个32位的整数型数值，即占用4个字节，能表示的数值个数为2的32次方，含正负号，范围为：-2147483648~2147483647
 - 从外部获得输入的变量值
 在 set 后面加上 /p 的参数，可以将变量设成用户输入的一行输入。读取输入行之前，显示指定的 提示文字。当然，提示文字也可以是空的。比如 set /p var=请输入一些文字： ，可以显示出一段提示文字"请输入一些文字："并能将用户输入的信息存到变量var里。/p 的参数还有很多诸如对字符串的替代、提取、增减等功能
 - 变量得赋值、显示、变换和计算
 把变量 var 里的值赋给另一个变量 var1 ，做法是：set var1=%var%
 echo var1 和 echo %var1% ，所得到的返回输出分别为：var1 和 Hello world! 。
输入的命令　　　　结果　　　　效果　　　　　　　　　　　　　　　　　　　
echo %var%　　　　1234567890　显示所有　　　　　　　　　　　　　　　　　
echo %var:~4%　　 567890　　　从第4个字符以后开始显示　　　　　　　　　
echo %var:~4,3%　 567　　　　 从第4个字符以后开始显示，并只显示前3个　　
echo %var:~-4%　　7890　　　　从倒数第4个字符开始显示　　　　　　　　　
echo %var:~-4,3%　789　　　　 从倒数第4个字符开始显示，并只显示前3个　　
echo %var:~4,-2%　5678　　　　从第4个字符以后开始显示，显示到还剩2个为止
echo %var:~0,3%　 123　　　　 从头开始显示，并只显示前3个字符　　　　　
echo %var:~0,-3%　1234567　　 从头开始显示，显示到还剩3个字符为止　　
###### 调用
 - 跳转
 goto 跟上标签就能直接让程序从该标签处开始继续执行随后的命令，不论标签的位置是在该 goto 命令的前面还是后面。标签必须以单个冒号 : 开头，但不区分大小写。有个特殊的标签 :EOF 或 :eof 能将控制转移到当前批脚本文件的结尾处，它是不需要事先定义的。
 - 调用
 call 主要体现在两个方面：一是调用该批处理以外的另一个批处理(事实上调用该批处理本身也可以，只是可能会带来不必要的死循环)；另一方面是有着与 goto 类似的向特定标签处跳转的功能。然而，call 的独特之处在于：在调用的批处理或标签后的内容处理完成以后，控制会继续执行 call 后面的语句
 - 启动
start 虽然也不是一个简单的命令，但用法绝对不难理解。来几个例子：start msconfig 用来打开"系统配置应用程序"；start notepad 则可以打开一个记事本；start "这就是所谓的标题" cmd 用来打开一个新的命令提示符；start "随便写个标题" http://www.baidu.com 便打开百度的首页；而 start "开玩了" E:\game\starcraft\starcraft.exe 却是开始星际争霸(如果您的电脑里安装了星际且路径与上述一致的话)等等。虽然 start 的参数很多(具体用法在输入 help start 后可以得到)，但通常情况下我们只需要知道 start 后面加上标题，再跟上想要执行程序、命令或网址即可。值得注意的是：标题要用双引号引用起来，否则会被作为可执行的文件来处理；所要执行的东西如果不是系统内部程序或命令的话，则需要我们给出具体的路径，比如绝对路径。
###### 参数
 - 参数传递
 :::::::::::被调用.bat:::::::::::
echo 这里是 被调用.bat
echo 您输入的第1条参数为 %1
echo 您输入的第2条参数为 %2
pause
::::::::::::::::::::::::::::::::
其中，%1 和 %2 分别代表运行"被调用.bat"批处理时所跟的两个参数。那么它该如何获得所谓的参数1和参数2呢。双击运行"被调用.bat"当然是不行的了。可以在命令提示符里输入"被调用.bat"的全名，并在后面加上两个参数即可(如果"被调用.bat"不在当前工作路径，需要输入"被调用.bat"的路径，比如绝对路径，以便让计算机找到它)。就像：D:\批处理\test\被调用.bat Tom and Jerry 。运行时我们会发现 %1 和 %2 分别显示为 Tom 和 and ，Jerry 为作为第3个参数来处理，但该批处理中却未用到 %3 。提示：在XP等操作系统中，对于汉字的输入可用 Ctrl + 空格 切换出中文输入法；也可以按 Tab 键让其自动切换并补充完成您所想要输入的路径。
 - 参数的输入与输出
 ::::::::::::调用.bat::::::::::::
@echo off
echo 这里是 调用.bat
pause

set /p Num=请输入一个整数:
set Square=

call 被调用.bat Num Square

echo 现在又回到了 调用.bat ，而且，%Num% 的平方是 %Square% 。
pause
::::::::::::::::::::::::::::::::

:::::::::::被调用.bat:::::::::::
echo 这里是 被调用.bat
echo 您输入的第1条参数为 %1
echo 您输入的第2条参数为 %2

set /a %2 = %1 * %1

echo 经过计算后，您输入的第1条参数为 %1
echo 经过计算后，您输入的第2条参数为 %2
pause
::::::::::::::::::::::::::::::::
 - 函数
::::::::::::跳转.bat::::::::::::
@echo off
call :FirstLable 很好很强大

:SecondLable
echo 然后显示这句
pause
goto :EOF

:FirstLable
echo 首先显示这句，后面跟的参数为 %1
pause
::::::::::::::::::::::::::::::::

对于 goto 所跟的标签或 start 所跟的程序，它们后面能否加参数呢？答案是：前者是否定的；后者是肯定的，试试看。
##### 条件/循环
###### 条件if
set var=Tom
if %var%\==Tom echo It works
if %var%\==Jerry echo We will never see this
如果变量 var 的值为 Tom Hanks ，即中间含有空格之类的特殊符号，那么我们在使用 if 时，就得为字符串加上双引号，就像 if "%var%"\=="Tom Hanks" echo It works (注意，给字符串加上双引号后，在进行判断的时候会连双引号一起考虑进去。所以，为了使两边的对比均衡，所以一定要在 == 两边的两个字符串上同时都加双引号)。这里也体现了批处理程序语言格式的多样性(如果您熟悉 C 语言格式的话，就知道一串字符总是要被双引号引起来)。不过为了方便记忆，我们在使用 if 的时候，不妨总是在字符串上使用双引号，这样既好阅读，又不容易引起歧异。
:::::::::else的用法.bat:::::::::
@echo off

if "%TIME:~0,2%" lss "12" (
echo 现在是上午
) else (
echo 现在是下午
)

pause
::::::::::::::::::::::::::::::::

:::::::::else的用法.bat:::::::::
@echo off

if "%TIME:~0,2%" lss "12" (
if "%TIME:~0,2%" lss " 6" (
echo 现在是凌晨
) else (
echo 现在是上午
)
) else (
if "%TIME:~0,2%" lss "18" (
echo 现在是下午
) else (
echo 现在是晚上
)
)

pause
::::::::::::::::::::::::::::::::
4.1.4 刚才提到的数值型变量的比较，其实很简单，就如下面的例子中描述的一样。
::::::::::::::::::::::::::::::::
@echo off
set /a num=5

if %num% == 5 (
echo 变量 num 等于 5
)

if not %num% == 4 (
echo 变量 num 不等于 4
)

set /a num = ( %num% + 3 ) * 2
:: 变量 num 加3并乘2后再赋给变量 num 自身

if %num% == 16 (
echo 经过运算后，现在变量 num 等于16
)

if not %num% == 16 (
echo 此时的变量 num 不会不等于 16 ，因此这一句不会显示了
)

pause
::::::::::::::::::::::::::::::::
###### 延迟变量
::::::::延迟变量扩充.bat::::::::
@echo off
setlocal EnableDelayedExpansion

set /a num=5

if %num% == 5 (
set /a num*=3

echo 在 if 语句之前，变量 num 等于 %num%
echo 但变量 num 在经过运算后，且由于延迟变量扩充被启用，变量 num 等于 !num!
)

echo 但最终变量 num 还是等于 %num%

pause
::::::::::::::::::::::::::::::::
if 条件下的两行 echo 在输出变量值的时候用到的符号不一样，一个是用百分号 % 包括起来的，另一个用的却是惊叹号 ! 。虽然在显示 %num% 之前已经使变量 num 的数值乘了3倍，但是由于没有延迟变量的扩充，使得 %num% 的结果仍然是 5 。但用 !num! 显示出的值已经变为 15 了。注意到批处理中的 setlocal EnableDelayedExpansion (setlocal/? 查看相关信息)，这表示开启延迟变量扩充。此时的 !num! 才有意义。不然 !num! 将无法被识别，因为在默认情况下，延迟变量扩充是被停用的。

4.1.6 此外，if 还有其他的用法—— if exist 和 if defined 。if exist 可判断文件是否存在，就像这样：
if exist "D:\test my folder\a.txt" (
del "D:\test my folder\a.txt"
) else (
echo 您所要删除的文件不存在
)
在对文件进行操作之前进行判断其是否存在很有意义，这使得代码更加健壮。

而对于 if defined 来说，与 if exist 类似，只不过 if defined 的判断对象不是文件，而是变量，它用于判断环境变量是否被定义。
###### 循环for
for %i in (*.*) do @echo %i
for 、in 和 do ，是 for 的固定用法。其内容可以理解为：在某一范围内(in)，对于其中的某一文件来说(for)，做如下的处理(do)。而 for %i in (*.*) do @echo %i 就是在当前工作目录的所有文件中(in (*.*))，对于其中的某一文件(for %i)，做出显示其名称的处理(do @echo %i)。变量 i 仅在当前循环语句 for 里起作用，%i 表示其值。
==注意：以上是直接在命令提示符里以命令的形式表达出来的写法；在批处理文件中应使用双百分号 %% 代替单百分比号 % ，就像：%%i==
:::::::批量修改文件名.bat:::::::
@echo off
setlocal EnableDelayedExpansion
set /a num=1
for %%i in (D:\test\*.txt) do (
ren "%%i" !num!.txt
set /a num+=1
)
::::::::::::::::::::::::::::::::
###### 参考
>[Windows 批处理脚本学习教程](http://docs.30c.org/dosbat/chapter03/)   
>[Windows批处理整理](http://localnetwork.cn/project-2/doc-148/)   
>[Python示例及教程](http://localnetwork.cn/project-4/doc-333/)   
>[window offical tools](https://learn.microsoft.com/zh-cn/sysinternals/)   
>
