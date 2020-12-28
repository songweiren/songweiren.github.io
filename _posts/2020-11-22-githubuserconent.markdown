---
layout: post
title:  "解决GitHub的raw.githubusercontent.com无法连接问题"
date:   2020-11-22 22:49:52 +0800
categories: github Linux
tags: config
---

修改hosts Ubuntu，CentOS及macOS直接在终端输入
```bash
sudo vi /etc/hosts
```
添加以下内容保存即可 

52.74.223.119 github.com

192.30.253.119 gist.github.com

54.169.195.247 api.github.com

185.199.111.153 assets-cdn.github.com

199.232.96.133 raw.githubusercontent.com

151.101.108.133 user-images.githubusercontent.com

151.101.76.133 gist.githubusercontent.com

151.101.76.133 cloud.githubusercontent.com

151.101.76.133 camo.githubusercontent.com

151.101.76.133 avatars0.githubusercontent.com

151.101.76.133 avatars1.githubusercontent.com

151.101.76.133 avatars2.githubusercontent.com

151.101.76.133 avatars3.githubusercontent.com

151.101.76.133 avatars4.githubusercontent.com

151.101.76.133 avatars5.githubusercontent.com

151.101.76.133 avatars6.githubusercontent.com

151.101.76.133 avatars7.githubusercontent.com

151.101.76.133 avatars8.githubusercontent.com
