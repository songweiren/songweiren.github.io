---
layout: post
title:  "常用正则表达式"
date:   2024-05-01 20:49:52 +0800
categories: C#  
tags: c#
---

整数或者小数
```
^[0-9]+\.{0,1}[0-9]{0,2}$ 
```
只能输入数字
```
^[0-9]*$

```
只能输入n位的数字
```
^\d{n}$
```
只能输入至少n位的数字
```
^\d{n,}$
```
只能输入m~n位的数字
```
^\d{m,n}$ 
```
只能输入零和非零开头的数字
```
^(0|[1-9][0-9]*)$
```
只能输入有两位小数的正实数
```
^[0-9]+(.[0-9]{2})?$

```
只能输入有1~3位小数的正实数
```
^[0-9]+(.[0-9]{1,3})?$
```
只能输入非零的正整数
```
^\+?[1-9][0-9]*$
```
只能输入非零的负整数
```
^\-[1-9][]0-9*$
```
只能输入长度为3的字符
```
^.{3}$
```
只能输入由26个英文字母组成的字符串
```
^[A-Za-z]+$
```
只能输入由26个大写英文字母组成的字符串
```
^[A-Z]+$
```
只能输入由26个小写英文字母组成的字符串
```
^[a-z]+$
```
只能输入由数字和26个英文字母组成的字符串
```
^[A-Za-z0-9]+$
```
只能输入由数字、26个英文字母或者下划线组成的字符串
```
^\w+$
```
验证用户密码：注：正确格式为：以字母开头，长度在6~18之间，只能包含字符、数字和下划线。
```
^[a-zA-Z]\w{5,17}$ 
```
验证是否含有^%&',;=?$\等字符
```
[^%&',;=?$\x22]+ 
```
只能输入汉字
```
^[\u4e00-\u9fa5]{0,}$ 
```
验证Email地址
```
^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
```
验证Internet URL
```
^[http|https]://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$
```
验证电话号码  :正确格式为：XXX-XXXXXXX、XXXX- XXXXXXXX、XXX-XXXXXXX、XXX-XXXXXXXX、XXXXXXX和XXXXXXXX  
```
^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$
```
验证身份证号（15位或18位数字）
```
^\d{15}|\d{18}$
```
验证一年的12个月:正确格式为：01～09和1～12
```
^(0?[1-9]|1[0-2])$
```
验证一个月的31天
```
^((0?[1-9])|((1|2)[0-9])|30|31)$
```
匹配中文字符的正则表达式
```
[\u4e00-\u9fa5]
```
匹配双字节字符(包括汉字在内)
```
[^\x00-\xff] 
```
匹配空行的正则表达式
```
\n[\s| ]*\r
```
匹配html标签的正则表达式
```
<(.*)>(.*)<\/(.*)>|<(.*)\/>

```
匹配首尾空格的正则表达式
```
(^\s*)|(\s*$)
```

匹配Email地址的正则表达式
```
\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*
```
匹配HTML标记的正则表达式
```
<(\S*?)[^>]*>.*?|<.*? />
```
匹配首尾空白字符的正则表达式
```
^\s*|\s*$
```
匹配Email地址的正则表达式
```
\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*
```
匹配网址URL的正则表达式
```
[a-zA-z]+://[^\s]*
```
匹配帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)
```
^[a-zA-Z][a-zA-Z0-9_]{4,15}$
```
匹配国内电话号码
```
\d{3}-\d{8}|\d{4}-\d{7}
```
匹配腾讯QQ号
```
[1-9][0-9]{4,}
```
匹配中国邮政编码
```
[1-9]\d{5}(?!\d)
```
匹配身份证
```
\d{15}|\d{18}

```
匹配ip地址
```
\d+\.\d+\.\d+\.\d+
```
匹配特定数字
```
^[1-9]\d*$ //匹配正整数
^-[1-9]\d*$ //匹配负整数
^-?[1-9]\d*$ //匹配整数
^[1-9]\d*|0$ //匹配非负整数（正整数 + 0）
^-[1-9]\d*|0$ //匹配非正整数（负整数 + 0）
^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$ //匹配正浮点数
^-([1-9]\d*\.\d*|0\.\d*[1-9]\d*)$ //匹配负浮点数
^-?([1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0)$ //匹配浮点数
^[1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0$ //匹配非负浮点数（正浮点数 + 0）
^(-([1-9]\d*\.\d*|0\.\d*[1-9]\d*))|0?\.0+|0$//匹配非正浮点数（负浮点数 + 0）s
评注：处理大量数据时有用，具体应用时注意修正。

```
匹配特定字符串
```
^[A-Za-z]+$//匹配由26个英文字母组成的字符串
^[A-Z]+$//匹配由26个英文字母的大写组成的字符串
^[a-z]+$//匹配由26个英文字母的小写组成的字符串
^[A-Za-z0-9]+$//匹配由数字和26个英文字母组成的字符串
^\w+$//匹配由数字、26个英文字母或者下划线组成的字符串

评注：最基本也是最常用的一些表达式
```
校验密码强度例如密码的强度为：包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间。
```
^(?=.*\\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$
```
校验字符串
```
//中文。
^[\\u4e00-\\u9fa5]{0,}$
//由数字、26个英文字母或下划线组成的字符串
^\\w+$
//校验E-Mail 地址
[\\w!#$%&'*+/=?^_`{|}~-]+(?:\\.[\\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\\w](?:[\\w-]*[\\w])?\\.)+[\\w](?:[\\w-]*[\\w])?
//校验身份证号码15位：
^[1-9]\\d{7}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}$
//18位
^[1-9]\\d{5}[1-9]\\d{3}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}([0-9]|X)$
//校验日期“yyyy-mm-dd“ 格式的日期校验，已考虑平闰年。
^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$
//校验金额精确到2位小数。
^[0-9]+(.[0-9]{2})?$
//校验手机号下面是国内 13、15、18开头的手机号正则表达式。（可根据目前国内收集号扩展前两位开头号码）
^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\\d{8}$
//判断IE的版本
^.*MSIE [5-8](?:\\.[0-9]+)?(?!.*Trident\\/[5-9]\\.0).*$
//校验IP-v4地址
\\b(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\b
//校验IP-v6地址
(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))



```

网页相关
```
//检查URL的前缀

//应用开发中很多时候需要区分请求是HTTPS还是HTTP，通过下面的表达式可以取出一个url的前缀然后再逻辑判断。
if (!s.match(/^[a-zA-Z]+:\\/\\//))
{
    s = 'http://' + s;
}
//下面的这个表达式可以筛选出一段文本中的URL。
^(f|ht){1}(tp|tps):\\/\\/([\\w-]+\\.)+[\\w-]+(\\/[\\w- ./?%&=]*)?
//文件路径及扩展名校验验证windows下文件路径和扩展名（下面的例子中为.txt文件）
^([a-zA-Z]\\:|\\\\)\\\\([^\\\\]+\\\\)*[^\\/:*?"<>|]+\\.txt(l)?$
//提取网页颜色代码有时需要抽取网页中的颜色代码，可以使用下面的表达式。
^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$
//提取网页图片
\\< *[img][^\\>]*[src] *= *[\\"\\']{0,1}([^\\"\\'\\ >]*)
//提取页面超链接
(<a\\s*(?!.*\\brel=)[^>]*)(href="https?:\\/\\/)((?!(?:(?:www\\.)?'.implode('|(?:www\\.)?', $follow_list).'))[^"]+)"((?!.*\\brel=)[^>]*)(?:[^>]*)>
//查找CSS属性
^\\s*[a-zA-Z\\-]+\\s*[:]{1}\\s[a-zA-Z0-9\\s.#]+[;]{1}
//抽取注释
<!--(.*?)-->
//匹配HTML标签
<\\/?\\w+((\\s+\\w+(\\s*=\\s*(?:".*?"|'.*?'|[\\^'">\\s]+))?)+\\s*|\\s*)\\/?>
//时间正则案例
//简单的日期判断（YYYY/MM/DD）
^\d{4}(\-|\/|\.)\d{1,2}\1\d{1,2}$ 
//演化的日期判断（YYYY/MM/DD| YY/MM/DD）
^(^(\d{4}|\d{2})(\-|\/|\.)\d{1,2}\3\d{1,2}$)|(^\d{4}年\d{1,2}月\d{1,2}日$)$ 
//加入闰年的判断的
^((((1[6-9]|[2-9]\d)\d{2})-(0?[13578]|1[02])-(0?[1-9]|[12]\d|3[01]))|(((1[6-9]|[2-9]\d)\d{2})-(0?[13456789]|1[012])-(0?[1-9]|[12]\d|30))|(((1[6-9]|[2-9]\d)\d{2})-0?2-(0?[1-9]|1\d|2[0-8]))|(((1[6-9]|[2-9]\d)(0[48]|[2468][048]|[13579][26])|((16|[2468][048]|[3579][26])00))-0?2-29-))$ 


```

参考
元字符
描述
\
将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如，“\\n”匹配\n。“\n”匹配换行符。序列“\\”匹配“\”而“\(”则匹配“(”。即相当于多种编程语言中都有的“转义字符”的概念。
^
匹配输入字行首。如果设置了RegExp对象的Multiline属性，^也匹配“\n”或“\r”之后的位置。
$
匹配输入行尾。如果设置了RegExp对象的Multiline属性，$也匹配“\n”或“\r”之前的位置。
*
匹配前面的子表达式任意次。例如，zo*能匹配“z”，也能匹配“zo”以及“zoo”。*等价于{0,}。
+
匹配前面的子表达式一次或多次(大于等于1次）。例如，“zo+”能匹配“zo”以及“zoo”，但不能匹配“z”。+等价于{1,}。
?
匹配前面的子表达式零次或一次。例如，“do(es)?”可以匹配“do”或“does”。?等价于{0,1}。
{n}
n是一个非负整数。匹配确定的n次。例如，“o{2}”不能匹配“Bob”中的“o”，但是能匹配“food”中的两个o。
{n,}
n是一个非负整数。至少匹配n次。例如，“o{2,}”不能匹配“Bob”中的“o”，但能匹配“foooood”中的所有o。“o{1,}”等价于“o+”。“o{0,}”则等价于“o*”。
{n,m}
m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次。例如，“o{1,3}”将匹配“fooooood”中的前三个o为一组，后三个o为一组。“o{0,1}”等价于“o?”。请注意在逗号和两个数之间不能有空格。
?
当该字符紧跟在任何一个其他限制符（*,+,?，{n}，{n,}，{n,m}）后面时，匹配模式是非贪婪的。非贪婪模式尽可能少地匹配所搜索的字符串，而默认的贪婪模式则尽可能多地匹配所搜索的字符串。例如，对于字符串“oooo”，“o+”将尽可能多地匹配“o”，得到结果[“oooo”]，而“o+?”将尽可能少地匹配“o”，得到结果 ['o', 'o', 'o', 'o']
.点
匹配除“\n”和"\r"之外的任何单个字符。要匹配包括“\n”和"\r"在内的任何字符，请使用像“[\s\S]”的模式。
(pattern)
匹配pattern并获取这一匹配。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“\(”或“\)”。
(?:pattern)
非获取匹配，匹配pattern但不获取匹配结果，不进行存储供以后使用。这在使用或字符“(|)”来组合一个模式的各个部分时很有用。例如“industr(?:y|ies)”就是一个比“industry|industries”更简略的表达式。
(?=pattern)
非获取匹配，正向肯定预查，在任何匹配pattern的字符串开始处匹配查找字符串，该匹配不需要获取供以后使用。例如，“Windows(?=95|98|NT|2000)”能匹配“Windows2000”中的“Windows”，但不能匹配“Windows3.1”中的“Windows”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。
(?!pattern)
非获取匹配，正向否定预查，在任何不匹配pattern的字符串开始处匹配查找字符串，该匹配不需要获取供以后使用。例如“Windows(?!95|98|NT|2000)”能匹配“Windows3.1”中的“Windows”，但不能匹配“Windows2000”中的“Windows”。
(?<=pattern)
非获取匹配，反向肯定预查，与正向肯定预查类似，只是方向相反。例如，“(?<=95|98|NT|2000)Windows”能匹配“2000Windows”中的“Windows”，但不能匹配“3.1Windows”中的“Windows”。
*python的正则表达式没有完全按照正则表达式规范实现，所以一些高级特性建议使用其他语言如java、scala等
(?<!pattern)
非获取匹配，反向否定预查，与正向否定预查类似，只是方向相反。例如“(?<!95|98|NT|2000)Windows”能匹配“3.1Windows”中的“Windows”，但不能匹配“2000Windows”中的“Windows”。
*python的正则表达式没有完全按照正则表达式规范实现，所以一些高级特性建议使用其他语言如java、scala等
x|y
匹配x或y。例如，“z|food”能匹配“z”或“food”(此处请谨慎)。“[z|f]ood”则匹配“zood”或“food”。
[xyz]
字符集合。匹配所包含的任意一个字符。例如，“[abc]”可以匹配“plain”中的“a”。
[^xyz]
负值字符集合。匹配未包含的任意字符。例如，“[^abc]”可以匹配“plain”中的“plin”任一字符。
[a-z]
字符范围。匹配指定范围内的任意字符。例如，“[a-z]”可以匹配“a”到“z”范围内的任意小写字母字符。
注意:只有连字符在字符组内部时,并且出现在两个字符之间时,才能表示字符的范围; 如果出字符组的开头,则只能表示连字符本身.
[^a-z]
负值字符范围。匹配任何不在指定范围内的任意字符。例如，“[^a-z]”可以匹配任何不在“a”到“z”范围内的任意字符。
\b
匹配一个单词的边界，也就是指单词和空格间的位置（即正则表达式的“匹配”有两种概念，一种是匹配字符，一种是匹配位置，这里的\b就是匹配位置的）。例如，“er\b”可以匹配“never”中的“er”，但不能匹配“verb”中的“er”；“\b1_”可以匹配“1_23”中的“1_”，但不能匹配“21_3”中的“1_”。
\B
匹配非单词边界。“er\B”能匹配“verb”中的“er”，但不能匹配“never”中的“er”。
\cx
匹配由x指明的控制字符。例如，\cM匹配一个Control-M或回车符。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的“c”字符。
\d
匹配一个数字字符。等价于[0-9]。grep 要加上-P，perl正则支持
\D
匹配一个非数字字符。等价于[^0-9]。grep要加上-P，perl正则支持
\f
匹配一个换页符。等价于\x0c和\cL。
\n
匹配一个换行符。等价于\x0a和\cJ。
\r
匹配一个回车符。等价于\x0d和\cM。
\s
匹配任何不可见字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。
\S
匹配任何可见字符。等价于[^ \f\n\r\t\v]。
\t
匹配一个制表符。等价于\x09和\cI。
\v
匹配一个垂直制表符。等价于\x0b和\cK。
\w
匹配包括下划线的任何单词字符。类似但不等价于“[A-Za-z0-9_]”，这里的"单词"字符使用Unicode字符集。
\W
匹配任何非单词字符。等价于“[^A-Za-z0-9_]”。
\xn
匹配n，其中n为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，“\x41”匹配“A”。“\x041”则等价于“\x04&1”。正则表达式中可以使用ASCII编码。
\num
匹配num，其中num是一个正整数。对所获取的匹配的引用。例如，“(.)\1”匹配两个连续的相同字符。
\n
标识一个八进制转义值或一个向后引用。如果\n之前至少n个获取的子表达式，则n为向后引用。否则，如果n为八进制数字（0-7），则n为一个八进制转义值。
\nm
标识一个八进制转义值或一个向后引用。如果\nm之前至少有nm个获得子表达式，则nm为向后引用。如果\nm之前至少有n个获取，则n为一个后跟文字m的向后引用。如果前面的条件都不满足，若n和m均为八进制数字（0-7），则\nm将匹配八进制转义值nm。
\nml
如果n为八进制数字（0-7），且m和l均为八进制数字（0-7），则匹配八进制转义值nml。
\un
匹配n，其中n是一个用四个十六进制数字表示的Unicode字符。例如，\u00A9匹配版权符号（&copy;）。
\p{P}
小写 p 是 property 的意思，表示 Unicode 属性，用于 Unicode 正表达式的前缀。中括号内的“P”表示Unicode 字符集七个字符属性之一：标点字符。
其他六个属性：
L：字母；
M：标记符号（一般不会单独出现）；
Z：分隔符（比如空格、换行等）；
S：符号（比如数学符号、货币符号等）；
N：数字（比如阿拉伯数字、罗马数字等）；
C：其他字符。
*注：此语法部分语言不支持，例：javascript。
\<
\>
匹配词（word）的开始（\<）和结束（\>）。例如正则表达式\<the\>能够匹配字符串"for the wise"中的"the"，但是不能匹配字符串"otherwise"中的"the"。注意：这个元字符不是所有的软件都支持的。
( )
将( 和 ) 之间的表达式定义为“组”（group），并且将匹配这个表达式的字符保存到一个临时区域（一个正则表达式中最多可以保存9个），它们可以用 \1 到\9 的符号来引用。
|
将两个匹配条件进行逻辑“或”（or）运算。例如正则表达式(him|her) 匹配"it belongs to him"和"it belongs to her"，但是不能匹配"it belongs to them."。注意：这个元字符不是所有的软件都支持的。







