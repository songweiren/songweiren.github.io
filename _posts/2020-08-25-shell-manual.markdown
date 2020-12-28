---
layout: post
title:  "Shell"
date:   2020-08-25 22:49:52 +0800
categories: Linux
tags: Shell
---
1 列出目录内容：ls

-a 显示所有文件，包括隐藏的

-l 长格式列出信息

-i 显示文件 inode 号

-t 按修改时间排序

-r 按修改时间倒序排序

2 打印：echo

3 格式化打印：printf

   用法：printf format [agrs]

format：

%.ns 输出字符串，n 是输出几个字符

%ni 输出整数，n 是输出几个数字

%m.nf 输出浮点数，m 是输出的整数位数，n 是输出的小数位数

%x 不带正负号的十六进制值，使用 a 至 f 表示 10 至 15

%X 不带正负号的十六进制，使用 A 至 F 表示 10 至 15

%% 输出单个%

 

一些常用的空白符：

\n 换行

\r 回车

\t 水平制表符

对齐方式：

%-5s 对参数每个字段左对齐，宽度为 5

%-4.2f 左对齐，宽度为 4，保留两位小数

不加横线"-"表示右对齐。

4 连接文件和标准输出打印：cat

-A 查看所有内容

-b 显示非空行行号

-n 显示所有行行号

-T 显示 tab，用^I 表示

-E 显示以$结尾

5 倒序打印：tac

6 复制文件或目录：cp

-a 归档

-b 目标文件存在创建备份，备份文件是文件名跟~

-f 强制复制文件或目录

-r 递归复制目录

-p 保留原有文件或目录属性

-i 覆盖文件之前先询问用户

-u 当源文件比目的文件修改时间新时才复制

-v 显示复制信息

7 创建目录：mkdir

-p 递归创建目录

-v 显示创建过程

8 移动文件或重命名

-b 目标文件存在创建备份，备份文件是文件名跟~

-u 当源文件比目的文件修改时间新时才移动

-v 显示移动信息

9 估算磁盘文件空间使用：du

-b 单位 bytes 显示

-c 产生一个总大小

-h 易读格式显示（K，M，G）

-k 单位 KB 显示

-m 单位 MB 显示

-s 只显示总大小

--max-depth=<目录层数>，超过层数的目录忽略

--exclude=file 排除文件或目录

--time 显示大小和创建时间

10 选取文件的每一行数据

-b 选中第几个字符、

-c 选中多少个字符

-d 指定分隔符，默认是空格

-f 指定显示选中字段

示例：

打印 b 字符：

# echo "abc" |cut -b "2"

b

截取 abc 字符：

# echo "abcdef" |cut -c 1-3

abc

已冒号分隔，显示第二个字段：

# echo "a:b:c" |cut -d: -f2

11 显示文件或文件系统状态：stat

12 打印序列化数字：seq

-f 使用 printf 样式格式

-s 指定分隔符，默认换行符\n

-w 等宽，用 0 填充

13 生成随机序列：shuf

常用选项：

-i 输出数字范围

-o 结果写入文件

示例：

输出范围随机数：

# seq 5 |shuf

2

1

5

4

3

14 排序文本：sort

-f 忽略大小写

-g 一般数字排序

-M 根据月份比较排序，比如 JAN、DEC

-h 易读的大小单位排序，比如 2K、1G

-n 数字比较排序

-r 倒序排序

-k n,m 根据关键字排序，从第 n 字段开始，m 字段结束

-o 将结果写入文件

-t 指定分隔符

-u 去重重复行

15 从标准输入读取写入标准输出和文件：tee

-a 追加到文件

示例：

打印并追加到文件：

# echo 123 |tee -a a.log

16 连接两个文件：join

-a <1 或 2> 除显示原来输出的内容外，还显示指定文件中没有相同的栏位，默认不显示

-i 忽略大小写

-o 按照指定文件栏位显示

-t 使用字符作为输入和输出字段分隔符

-1 连接文件 1 的指定栏位

-2 连接文件 2 的指定栏位

17 合并文件：paste

-d 指定分隔符，默认是 tab 键

-s 将文件内容平行，tab 键分隔

18 输出文件前几行：head

-c 打印前多少 K、bytes

-n 打印前多少行

19 输出文件后几行：tail

-c 打印前多少 K、bytes

-f 实时读文件，随着文件输出附加输出

-n 输出最后几行

--pid 与-f 一起使用，表示 pid 死掉后结束

-s 与-f 一起使用，表示休眠多少秒输出

20 搜索目录文件层次结构：find

格式：find path -option actions

常用选项：

-name 文件名，支持(‘*’, ‘?’, and ‘[]’)

-type 文件类型，d 目录，f 常规文件等

-perm 符合权限的文件，比如 755

-atime -/+n 在 n 天以内/过去 n 天被访问过

-ctime -/+n 在 n 天以内/过去 n 天被修改过

-amin -/+n 在 n 天以内/过去 n 分钟被访问过

-cmin -/+n 在 n 天以内/过去 n 分钟被修改过

-size -/+n 文件大小小于/大于，b、k、M、G

-maxdepth levels 目录层次显示的最大深度

-regex pattern 文件名匹配正则表达式模式

-inum 通过 inode 编号查找文件

动作：

-detele 删除文件

-exec command {} \; 执行命令，花括号代表当前文件

-ls 列出当前文件，ls -dils 格式

-print 完整的文件名并添加一个回车换行符

-print0 打印完整的文件名并不添加一个回车换行符

-printf format 打印格式

示例：

查找文件名：

# find / -name "*http*"

查找文件名并且文件类型：

# find /tmp -name core -type f -print

查找文件名并且文件类型删除：

# find /tmp -depth -name core -type f -delete

查找当前目录常规文件并查看文件类型：

# find . -type f -exec file '{}' \;

查找文件权限是 664：

# find . -perm 664

查找大于 1024k 的文件：

# find . -size -1024k

查找 3 天内修改的文件：

# find /bin -ctime -3

排除多个类型的文件：

# find . ! -name "*.sql" ! -name "*.txt"

或条件查找多个类型的文件：

# find . -name '*.sh' -o -name '*.bak'

# find . -regex ".*\.sh|.*\.bak"

# find . -regex ".*\.sh∥bak"

并且条件查找文件：

# find . -name "*.sql" -a -size +1024k

只显示第一级目录：

# find /etc -type d -maxdepth 1

通过 inode 编号删除文件：

# rm `find . -inum 671915`

# find . -inum 8651577 -exec rm -i {} \;