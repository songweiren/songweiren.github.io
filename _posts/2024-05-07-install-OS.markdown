---
layout: post
title:  "網絡啓動安裝操作系統"
date:   2024-05-07 8:00:00 +0800
categories: IT  
tags: IT/小技巧
---
### 通過網絡引導啓動安裝Windows10
要通过网络引导启动安装 Windows 10，你可以使用 Windows 部署服务（Windows Deployment Services，WDS）。WDS 是一种基于网络的安装服务，可用于自动化安装 Windows 操作系统。以下是设置 WDS 并通过网络引导启动安装 Windows 10 的简要步骤：

#### 准备环境：
- 在 Windows Server 上安装 WDS 角色。
- 准备一个包含 Windows 10 安装文件的映像（镜像）。
#### 配置 WDS：
- 打开服务器管理器，在 "角色和功能" 中选择 "添加角色和功能"。
- 选择 "Windows 部署服务" 角色，并按照向导进行安装。
- 在 WDS 管理器中，右键单击服务器名称，选择 "配置服务器"。
 -按照向导设置服务器，选择映像（镜像）的位置和其他参数。
#### 添加映像：
- 将准备好的 Windows 10 安装映像添加到 WDS 服务器中。
#### 配置引导：
- 在 WDS 管理器中，右键单击 "引导映像"，选择 "新建引导映像"。
- 选择引导映像的类型，并指定启动程序和初始化文件。
#### 引导客户端：
- 确保客户端计算机处于网络环境中，并在 BIOS 或 UEFI 设置中启用网络引导。
- 在引导时，选择网络引导，并选择相应的 WDS 服务器。
#### 选择安装映像：
- 在 WDS 引导菜单中，选择要安装的 Windows 10 映像。
#### 执行安装：
- 客户端计算机将从网络引导到 WDS 服务器，并自动执行 Windows 10 安装过程。
通过以上步骤，你可以通过网络引导启动并安装 Windows 10。确保在设置和配置过程中遵循安全最佳实践，并根据需要进行网络和访问控制配置，以确保系统安全。

### 通過網絡引導啓動安裝Ubuntu20

要通过网络引导启动并安装 Ubuntu 20.04，你可以使用 PXE（Preboot Execution Environment）和 TFTP（Trivial File Transfer Protocol）来设置一个自动化的安装环境。以下是一个简要的步骤：

#### 准备环境：
在网络中设置一个运行 DHCP 和 TFTP 服务器的计算机，这将充当 PXE 服务器。
在该服务器上安装 TFTP 服务器软件（如 TFTP-HPA 或 atftpd）和 DHCP 服务器软件（如 ISC DHCP Server）。
获取 Ubuntu 安装文件：
下载 Ubuntu 20.04 ISO 文件并将其挂载到一个目录中。
#### 配置 DHCP 服务器：
编辑 DHCP 服务器的配置文件，指定 PXE 启动文件的位置以及其他 PXE 相关参数。以下是一个示例配置：
```
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 192.168.1.1;

    next-server 192.168.1.10;    # PXE 服务器的 IP 地址
    filename "pxelinux.0";
}
```
#### 配置 TFTP 服务器：
将 Ubuntu 20.04 安装文件的引导文件和内核文件复制到 TFTP 服务器的根目录下。
#### 创建 PXE 引导文件：
在 TFTP 服务器的根目录下创建一个名为 pxelinux.cfg 的目录，并在其中创建一个名为 default 的文件。编辑 default 文件以指定 Ubuntu 安装过程的参数。例如：

```bash
DEFAULT install
LABEL install
    KERNEL ubuntu-installer/amd64/linux
    APPEND vga=788 initrd=ubuntu-installer/amd64/initrd.gz url=http://192.168.1.10/ubuntu20.04/preseed.cfg
```
其中，KERNEL 和 INITRD 指定了引导内核和初始 RAM 磁盘文件的位置，URL 指定了预配置文件的位置。
#### 创建预配置文件：
创建一个名为 preseed.cfg 的预配置文件，其中包含 Ubuntu 安装过程的自动化参数。你可以参考 Ubuntu 的官方文档以及网络上的教程来创建自定义的预配置文件。
#### 引导客户端：
在客户端计算机的 BIOS 或 UEFI 设置中启用网络引导，并选择 PXE 启动。
客户端计算机将从网络引导到 PXE 服务器，并自动开始 Ubuntu 20.04 的安装过程。
通过以上步骤，你可以设置一个基于网络引导的自动化 Ubuntu 20.04 安装环境。确保在设置过程中遵循安全最佳实践，并根据需要进行网络和访问控制配置，以确保系统安全。
