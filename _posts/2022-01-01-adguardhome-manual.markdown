---
layout: post
title:  "ADGuardHome Manual"
date:   2022-01-01 15:49:52 +0800
categories: Windows  
tags: Windows
---
### Windows

#### Installation 

1. .\AdGuardHome.exe --service install

2. Open Control Panel through Start menu or Windows search.

3. Go to Network and Internet category and then to Network and Sharing Center.

4. On the left side of the screen find “Change adapter settings” and click on it.

5. Select your active connection, right-click on it and choose Properties.

6. Find “Internet Protocol Version 4 (TCP/IPv4)” (or, for IPv6, “Internet Protocol Version 6 (TCP/IPv6)”) in the list, select it and then click on Properties again.

7. Choose “Use the following DNS server addresses” and enter your AdGuard Home server addresses.

#### Configuration 

以浏览国内网站为主的用户可以使用 anti-AD + Halflife 过滤规则，如有浏览国外网站的需要，可以根据需要添加 AdGuard DNS Filter、Fanboy's Annoyances List 等规则。不同规则之间会存在重叠的情况，可以通过 AdGuard Home 的拦截日志分析哪些规则的使用频率最高，哪些规则拦截频率最低，再加以取舍。

| 名称                             | 简介                                                         | 地址                                                         |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **AdGuard DNS Filter**           | AdGuard 官方维护的广告规则，涵盖多种过滤规则                 | https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_15_DnsFilter/filter.txt |
| **EasyList**                     | Adblock Plus 官方维护的广告规则                              | https://easylist-downloads.adblockplus.org/easylist.txt      |
| **EasyList China**               | 面向中文用户的 EasyList 去广告规则                           | https://easylist-downloads.adblockplus.org/easylistchina.txt |
| **EasyPrivacy**                  | 反隐私跟踪、挖矿规则                                         | https://easylist-downloads.adblockplus.org/easyprivacy.txt   |
| **Halflife 规则**                | 涵盖了 EasyList China、EasyList Lite、CJX ’s Annoyance、乘风视频过滤规则，以及补充的其它规则 | https://gitee.com/halflife/list/raw/master/ad.txt            |
| **Xinggsf 乘风过滤**             | 国内网站广告过滤规则                                         | https://gitee.com/xinggsf/Adblock-Rule/raw/master/rule.txt   |
| **Xinggsf 乘风视频过滤**         | 视频网站广告过滤规则                                         | https://gitee.com/xinggsf/Adblock-Rule/raw/master/mv.txt     |
| **MalwareDomainList**            | 恶意软件过滤规则                                             | https://www.malwaredomainlist.com/hostslist/hosts.txt        |
| **Adblock Warning Removal List** | 去除禁止广告拦截提示规则                                     | https://easylist-downloads.adblockplus.org/antiadblockfilters.txt |
| **Anti-AD**                      | 命中率高、兼容性强                                           | https://anti-ad.net/easylist.txt                             |
| **Fanboy’s Annoyances List**     | 去除页面弹窗广告规则                                         | https://easylist-downloads.adblockplus.org/fanboy-annoyance.txt |

DNS 服务推荐

不同地区连接至 DNS 服务器的速度各有差异，各位可以通过 Ping 测速的方式寻找当地连接延迟最低的 DNS 服务器。更多 DNS 服务器可以在 [AdGuard 文档](https://kb.adguard.com/zh/general/dns-providers)中找到。

#### 常见问题

##### 端口冲突

在 Linux 设备上运行 AdGuard Home，通常会出现 53（本地 DNS 服务器）、68（DHCP 客户端）、80（Http）、443（Https） 端口冲突的问题，可以通过 netstat -tunlp | grep 端口号 查询占用进程。有两种解决方案：使用不同端口、停用冲突进程。

如果是通过 Docker 方式运行 AdGuard Home，出现 `listen udp 0.0.0.0:53: bind: address already in use` 的提示，需要手动处理，方法如下

```
#停止 DNSStubListener
systemctl stop systemd-resolved

#创建文件夹（如果不存在）
mkdir /etc/systemd/resolved.conf.d/

#使用 Nano 创建配置文件
nano /etc/systemd/resolved.conf.d/adguardhome.conf
```

在编辑器中粘贴以下内容：

```
[Resolve]
DNS=127.0.0.1
DNSStubListener=no
```

保存后执行以下命令。

```
#创建备份
sudo mv /etc/resolv.conf /etc/resolv.conf.backup

#将 /etc/resolv.conf 链接至 /run/systemd/resolve/resolv.conf
ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf

#重启 DNSStubListener
systemctl restart systemd-resolved
```

完成后使用 `netstat -tunlp | grep 53` 命令检查是否依旧有进程占用 53 端口，如无冲突，重启 AdGuard Home 容器即可

##### 部分网页被 AdGuard Home 误杀

如果一些网页被 AdGuard Home 误杀，可以在 AdGuard Home 的日志寻找是否被拦截。如果与规则发生冲突，需要将误杀网址通过自定义过滤规则添加至白名单中，或选择其它的过滤规则。常见的冲突有网站统计服务（Google Analytics）、广告联盟等

##### 自定义过滤规则

AdGuard Home 的过滤规则兼容 Adblock 语法、Hosts 语法及 Domain-only 语法

| **语法**                | **作用**                                  |
| ----------------------- | ----------------------------------------- |
| `||example.org^`        | 拦截 example.org 域名及其所有子域名       |
| `@@||example.org^`      | 放行 example.org 及其所有子域名           |
| `127.0.0.1 example.org` | 将 example.org 解析到 127.0.0.1           |
| `/REGEX/`               | 阻止访问与 example_regex_meaning 匹配的域 |
| `! 这是一行注释`        | 只是一条注释                              |
| `# 这是一行注释`        | 只是一条注释                              |

##### 能否将 AdGuard Home DNS 与 Surge / Clash 网关结合使用？

可以。Surge 与 Clash 分别提供了 `dns-server` 与 `dns-nameserver` 字段以供用户修改 DNS 解析服务器，在配置文件中填入 AdGuard Home 的 DNS 服务器地址即可。