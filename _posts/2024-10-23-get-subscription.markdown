---
layout: post
title:  "获取V2ay链接"
date:   2024-10-23 08:49:52 +0800
categories: bat  
tags: bat
---
- 获取订阅链接内容：订阅链接通常是一个包含多个 V2Ray 配置的 Base64 编码字符串。你可以使用命令行工具（如 curl 或 wget）下载订阅链接内容。
``` bash
curl -o subscription.txt "https://your-subscription-link"
```
- 解码 Base64 内容：使用 Base64 解码工具解码下载的内容。
``` bash
base64 -d subscription.txt > decoded_subscription.txt
```
- 解析配置：解码后的文件包含多个 V2Ray 配置，你可以手动解析这些配置并添加到你的 V2Ray 客户端中。

