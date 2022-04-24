---
layout: post
title:  "V2ray Manual"
date:   2022-04-24 15:49:52 +0800
categories: Windows  
tags: Windows
---
#### 安装V2ray

---

输入下面命令回车

```bash
bash <(curl -s -L https://git.io/v2ray.sh)
```

> 如果提示 curl: command not found ，那是因为你的 VPS 没装 Curl
> ubuntu/debian 系统安装 Curl 方法: `apt-get update -y && apt-get install curl -y`
> centos 系统安装 Curl 方法: `yum update -y && yum install curl -y`
> 安装好 curl 之后就能安装脚本了

然后选择安装，即是输入 1 回车
选择传输协议，如果没有特别的需求，使用默认的 TCP 传输协议即可，直接回车
选择端口，如果没有特别的需求，使用默认的端口即可，直接回车
是否屏蔽广告，除非你真的需要，一般来说，直接回车即可

![v2ray1](..\image\v2ray1.jpg)

是否配置 Shadowsocks ，如果不需要就直接回车，否则就输入 Y 回车
Shadowsocks 端口，密码，加密方式这些东西自己看情况配置即可，我个人当然是全部直接回车。。
OK，按回车继续

![v2ray2](..\image\v2ray2.jpg)

安装信息，如果确保没有什么问题了，按回车继续

![v2ray3](..\image\v2ray3.jpg)

(备注，安装信息会因你的配置而变化..不用在乎这截图)
(备注，由于我懒…脚本显示的一些信息可能会跟上面的截图有少许不同，但实际上都是很简单明了的)

#### V2ray安装完成

---

OK，此时 V2Ray 已经安装完成了。

![v2ray4](..\image\v2ray4.jpg)

如上图所示，V2Ray 配置信息，Shadowsocks 配置信息都有了
如果你使用过 Shadowsocks ，那么现在你可以测试一下 Shadowsocks 配置了，看看是否能正常使用。
如果你使用过 V2Ray 某些客户端，那么现在也可以测试一下配置了。
(备注，可能某些 V2Ray 客户端的选项或描述略有不同，但事实上，上面的 V2Ray 配置信息已经足够详细，由于客户端的不同，请对号入座。)

#### V2Ray 客户端使用教程

由于 V2Ray 已经安装完成了，所以此时你应该尝试使用 V2Ray 来连接上真正的互联网了。

[Windows教程](https://github.com/233boy/v2ray/wiki/V2RayN%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B)

#### V2Ray 管理面板

---

现在可以尝试一下输入 `v2ray` 回车，即可管理 V2Ray

![v2ray5](..\image\v2ray5.jpg)

#### TCP 阻断

---

如果你觉得你的小鸡出现了这种情况，那么可以尝试使用 UDP 协议相关的 mKCP
当然，用了我的脚本那是很简单的啦，直接输入 `v2ray config` 然后选择修改 V2Ray 传输协议
之后再选择 mKCP 相关的就行咯
备注：使用 mKCP 或许还可以提高速度，但由于 UDP 的原因也许会被运营商 Qos，这是无解的。

#### 快速管理 V2Ray

---

`v2ray info` 查看 V2Ray 配置信息
`v2ray config` 修改 V2Ray 配置
`v2ray link` 生成 V2Ray 配置文件链接
`v2ray infolink` 生成 V2Ray 配置信息链接
`v2ray qr` 生成 V2Ray 配置二维码链接
`v2ray ss` 修改 Shadowsocks 配置
`v2ray ssinfo` 查看 Shadowsocks 配置信息
`v2ray ssqr` 生成 Shadowsocks 配置二维码链接
`v2ray status` 查看 V2Ray 运行状态
`v2ray start` 启动 V2Ray
`v2ray stop` 停止 V2Ray
`v2ray restart` 重启 V2Ray
`v2ray log` 查看 V2Ray 运行日志
`v2ray update` 更新 V2Ray
`v2ray update.sh` 更新 V2Ray 管理脚本
`v2ray uninstall` 卸载 V2Ray

#### 优化 V2Ray

---

由于本人的脚本在 Debian9 系统会自动开启 BBR 优化加速了，所以不需要再安装 BBR 优化了，
如果你还是觉得网络比较慢的话，你可以尝试使用含有 mKCP 的传输协议，这个 mKCP 的东东，简单一点说就像 Kcptun 一样加速，并且还能进行伪装 (可选)，但是由于 mKCP 是使用 UDP 协议的，也许运营商会限速得更加厉害，网络变得更加慢。但不管怎么样，你都可以随时尝试一下。
服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，再接着选择 mKCP 相关的传输协议即可
如果你是使用其他商家的 VPS 并且是按照此教程流程来安装 V2Ray 的话，那么你可以输入 `v2ray bbr` 回车，然后选择安装 BBR 或者 锐速 来优化 V2Ray
只是还想再啰嗦一下，如果你是使用国际大厂的 VPS，并且是按照此教程流程来安装 V2Ray 的话，请自行在安全组 (防火墙) 开放端口和 UDP 协议 (如果你要使用含有 mKCP 的传输协议)

#### WebSocket + TLS

---

实现 WebSocket + TLS 超级无敌简单，前提是要拥有一个能正常解析的域名 (并且知道怎么解析域名)
服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，再选择 WebSocket + TLS，即是输入 4，接着输入你的域名，然后我都懒得说了，脚本都那么简单明了，我还瞎BB干嘛…
哈哈…可能有不少人在折腾 V2Ray 实现 WS + TLS 的时候真的是搞到很蛋痛咯，有些人的教程可能说得不是很清楚，或者是直接忽略小白萌新这些亲爱的用户，嗯，小白们好好加油吧，请尽量多学一些基础知识，别总是做伸手党，对于毫无交集的陌生人，人家并没有任何义务要帮你的啊
偷偷跟你说…使用 WebSocket + TLS 会有断流的问题
多说一句，不要被某些人带节奏，WS + TLS 并不是 V2Ray 的神级配置，该墙还是会墙，墙你不需要理由
备注一下啦，这里我没写怎么教你注册域名啦，怎么解析域名啦，如果你真的想要使用 WebSocket + TLS，那就 自己谷歌摸索一下，其实好简单的啦！
我本人并没有在使用 WS + TLS (WebSocket + TLS)，我用 TCP，就是用一键脚本全程回车的那种懒人

## HTTP/2

实现 HTTP/2 (h2) 也超级无敌简单，和 WebSocket + TLS 一样，也就是只要一个域名就够
服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，再选择 HTTP/2，即是输入 16，然后………看上面的 WebSocket + TLS 的相关。
备注一下，HTTP/2 相比 WS + TLS (WebSocket + TLS) ，在浏览网页时有一些优势。速度是差不多的啦

## mKCP

mKCP 这个东东其实就是 KCP 协议，反正你知道是能提速的就行，但是不保证都能提速，还能避免 TCP 阻断，但是也可以会被运营商 Qos.
使用方法：服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，之后再选择 mKCP 相关的就行

#### Telegram 专用代理

---

> 重要提醒：不建议使用 V2Ray 的 MTProto 代理！
> 推荐使用： https://github.com/cutelua/mtg-dist



#### 哪个传输协议好？

心中无杂念，用 TCP
ISP 常作怪，用 动态端口
小鸡速度不好，用 mKCP
处子之身，用 WS + TLS

#### V2Ray 脚本说明

[V2ray一键安装脚本](https://github.com/233boy/v2ray/wiki/V2Ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC)