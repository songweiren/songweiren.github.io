---
layout: post
title:  "Linux下搭建SS"
date:   2020-11-22 22:49:52 +0800
categories: VPN Linux
tags: ss/ssr
---

首先root用户登陆
```bash
sudo -i
```
- 一、没有安装wget的先安装wget
  ```bash
  sudo yum -y install wget
  ```
- 二、下载一间安装脚本
```bash
  mkdir ss_pack  #建立文件夹 
  cd ss_pack     #切换到文件夹

  wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
```
- 三、赋予脚本执行权限
  ```bash
  chmod +x shadowsocks.sh
  ```
- 四、执行安装
  ```bash
  ./shadowsocks.sh 2>&1 | tee shadowsocks.log
  ```
  安装过程会会提示输入端口号，默认9571，可以设置为这个，还会提示输入密码。最后安装完成以后会显示ss server的ip端口号以及密码等信息，要记住这些东西，在客户端配置需要用到。
- 五、多用户配置
  以上配置为单端口，ss支持多端口配置使用。需修改配置文件，路径 为： /etc/shadowsocks.json
  ```bash
  vim /etc/shadowsocks.json
  ```

  默认的文件如下：
  ```bash
  {
   "server":"0.0.0.0",
   "server_port":9571,
   "local_address":"127.0.0.1",
   "local_port":1080,
   "password":"你自己设置的密码",
   "timeout":300,
   "method":"aes-256-cfb",
   "fast_open":false
  }

  ```
按 i 进入编辑模式，

我们可修改为如下，便是多端口配置：

```bash
{
   "server":"0.0.0.0",
   "server_port":9571,           #可以注释掉，也可以保留
   "local_address":"127.0.0.1",
   "local_port":1080,
   "password":"你自己设置的密码", #可以注释掉，也可以保留
   "port_password":{
     "9000":"addfgfdg",    #可以按照这样的格式写多个端口号
     "9001":"fwefewfe",    #9000为端口号，addfgfdg为对应端口号的密码
     "9002":"fdjhgjjj"
    }
   "timeout":300,
   "method":"aes-256-cfb",
   "fast_open":false
}
```
修改好后按esc 输入:wq 保存并推出

修改好后重启ss 
