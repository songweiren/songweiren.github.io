---
layout: post
title:  "Windows下小狼毫输入法（Rime）的安装与配置（含导入搜狗词库）"
date:   2022-08-31 8:49:52 +0800
categories:  Windows
tags: Rime
---
 首先去 http://rime.im 下载小狼毫输入法的安装程序进行安装：
    安装好后设置，我只选择了“朙月拼音”和“朙月拼音简化字”两种输入法，
##### （一）繁体转简体

 刚装好的时候一输入就傻眼了，居然输入的是繁体中文，这时候就需要用“Ctrl+`”，然后选择第5项，繁体中文转简体中文，这样就可以输入简体中文了。

##### （二）设置候选词个数

Rime的配置文件使用yaml写成的，放在用户目录的AppData\Roaming\Rime目录下。要修改候选词个数，需要新建文件default.custom.yaml，这个文件就是对default.yaml的修订，在其中加入如下的内容：
```
patch:
  "menu/page_size": 8

```
然后进行重新部署即可：
##### （三）设置候选词横排显示和字体
编辑weasel.custom.yaml，加入下面的内容：
```
patch:
  "style/color_scheme": google_plus
  "style/display_tray_icon": false
  "style/font_face": "Microsoft YaHei"
  "style/font_point": 13
  "style/horizontal": true

```
##### （四）导入搜狗词库

小狼毫自带的词库感觉还是不够给力，于是我决定导入搜狗的词库。首先要下载搜狗的标准词库： http://pinyin.sogou.com/dict/detail/index/11640

然后使用“深蓝词库转换工具”将这个scel文件转为Rime词库格式的txt文件，
https://github.com/studyzy/imewlconverter/releases
接下来将这个文件重命名为 luna_pinyin.sogou.dict.yaml ，并在文件内容最前面加入如下内容：
```
# Rime dictionary
# encoding: utf-8

---
name: luna_pinyin.sogou
version: "2015.12.24"
sort: by_weight
use_preset_vocabulary: true
import_tables:
  - luna_pinyin
...

```
接下来新建luna_pinyin_simp.custom.yaml，文件名表示针对朙月拼音的简化字输入方案，文件内容如下：
```
patch:
  translator/dictionary: luna_pinyin.sogou
# / symbols
  punctuator/import_preset: symbols
  recognizer/patterns/punct: '^/([0-9]0?|[A-Za-z]+)$'
```
然后用"Ctrl+`"，选择朙月拼音简化字，接下来重新部署小狼毫，就可以使用搜狗词库了。

##### 参考
> [https://blog.csdn.net/qq_46207024/article/details/114411457](https://blog.csdn.net/qq_46207024/article/details/114411457)
> [https://blog.csdn.net/qq_46207024/article/details/114411457](https://blog.csdn.net/qq_46207024/article/details/114411457)
> [https://sspai.com/post/55699](https://sspai.com/post/55699)
