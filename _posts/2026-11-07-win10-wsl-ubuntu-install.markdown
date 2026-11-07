---
layout: post
title:  "Windows 10 安装 WSL Ubuntu（在线 / 离线）"
date:   2026-11-07 00:30:00 +0800
categories: Windows
tags: Windows WSL Ubuntu
---

### 简介

WSL（Windows Subsystem for Linux）可在 Windows 10 上直接运行 Ubuntu，无需另装虚拟机软件。推荐使用 **WSL 2**。

本文覆盖：

- 安装前检查
- **在线安装**（一键命令）
- **离线安装**（WSL 本体 MSI + Ubuntu `.wsl`）
- 迁移发行版位置、常用命令与排错
- apt 软件源说明（默认非国内源；Ubuntu 24.04 / 22.04 换源）

系统要求：Windows 10 **2004** 及以上（Build **19041+**）。可用 `Win + R` → `winver` 查看。

---

### 安装前准备

#### 1. 开启 CPU 虚拟化（BIOS）

WSL 2 依赖硬件虚拟化。重启进入 BIOS/UEFI，开启：

- Intel：`Intel Virtualization Technology` / `VT-x`
- AMD：`SVM Mode`

回到系统后：任务管理器 → **性能** → **CPU**，确认「虚拟化」为 **已启用**。

> BIOS 里的 CPU 虚拟化一般不能靠 Windows 命令行打开；下面能用命令行开的是 Windows 可选功能。

#### 2. 用命令行开启 Windows 虚拟化相关功能

管理员 PowerShell：

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

完成后**重启**。

PowerShell 等价写法：

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux -All
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -All
```

---

### 在线安装（推荐，能联网时）

#### 方法一：一键安装

管理员 PowerShell：

```powershell
wsl --install
```

该命令会：启用必要功能、安装 WSL 内核、默认使用 WSL 2、安装 Ubuntu。按提示**重启**后，打开 Ubuntu，设置 Linux 用户名与密码（输入密码时不回显）。

#### 方法二：指定 Ubuntu 版本

```powershell
wsl --list --online
wsl --install -d Ubuntu-22.04
# 或
wsl --install -d Ubuntu-24.04
```

商店下载卡住时：

```powershell
wsl --install --web-download -d Ubuntu-22.04
```

> 若本机已装过 WSL，再执行 `wsl --install` 可能只显示帮助，改用 `wsl --install -d <发行版名>`。

#### 验证

```powershell
wsl -l -v
```

`VERSION` 应为 `2`。若为 `1`：

```powershell
wsl --set-version Ubuntu 2
```

进入系统后：

```bash
uname -a
lsb_release -a
sudo apt update && sudo apt upgrade -y
```

> 官方镜像默认多为**国际 apt 源**，国内较慢可先换源再 update，见下文「apt 软件源」。Ubuntu 24.04 改 `/etc/apt/sources.list.d/ubuntu.sources`。

---

### 离线安装

适合内网机、商店不可用、或用同事拷贝的安装包。需要两类包：

| 包 | 作用 | 大概体积 | 下载处 |
|----|------|----------|--------|
| `wsl.*.x64.msi` | **WSL 本体** | 约 200+ MB | [GitHub microsoft/WSL Releases](https://github.com/microsoft/WSL/releases) |
| `ubuntu-*-wsl-amd64.wsl` | **Ubuntu 镜像** | 约 300–400 MB | [Ubuntu WSL 下载页](https://ubuntu.com/download/wsl) / [releases.ubuntu.com](https://releases.ubuntu.com/) |

#### 别和下错包

| 正确 | 错误 / 易混 |
|------|-------------|
| `wsl.3.0.3.0.x64.msi`（本体，大） | `wsl_update_x64.msi` / wsl-update（**仅内核更新，约十几 MB**，不能当本体） |
| `ubuntu-22.04.5-wsl-amd64.wsl` | `*-desktop-amd64.iso`、`*-live-server-amd64.iso`（那是安装光盘，不是 WSL 镜像） |

> 旧文档里的「内核更新包」`wsl_update_x64.msi` 属于早期手动流程。有新版 **`wsl.*.x64.msi` 本体时，一般不必再装十几 MB 的 update**。

#### 离线步骤

**① 开启可选组件并重启**

管理员 PowerShell（二选一）：

```powershell
# 方式 A：只装 WSL 组件，不装发行版（适合后续离线装 Ubuntu）
wsl --install --no-distribution

# 方式 B：手动开启功能
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

完成后**必须重启**。若未开组件就执行 `wsl -l -v`，会报：

`Wsl/WSL_E_WSL_OPTIONAL_COMPONENT_REQUIRED`（缺少「适用于 Linux 的 Windows 子系统」可选组件）。

**② 安装 WSL 本体 MSI**

双击 `wsl.x.x.x.x.x64.msi`，或：

```powershell
msiexec /i "E:\离线包\wsl.3.0.3.0.x64.msi"
```

确认：

```powershell
wsl --version
```

**③ 设置默认版本为 WSL 2**

新版 MSI 多数已默认 WSL 2，但离线/旧机仍建议显式执行一次，再装发行版：

```powershell
wsl --set-default-version 2
```

> 该命令影响**之后新装**的发行版默认版本；已装成 WSL 1 的需再用 `wsl --set-version <发行版名> 2` 转换。

**④ 安装 Ubuntu 镜像**

- 双击 `ubuntu-22.04.5-wsl-amd64.wsl`，或：

```powershell
wsl --install --from-file "E:\离线包\ubuntu-22.04.5-wsl-amd64.wsl"
```

若文件无 `.wsl` 后缀、实际是可 import 的根文件系统，可用（此处已用 `--version 2` 明确指定）：

```powershell
mkdir D:\WSL\Ubuntu-22.04
wsl --import Ubuntu-22.04 D:\WSL\Ubuntu-22.04 "E:\离线包\Ubuntu-22.04.5-wsl-amd64" --version 2
```

`--import` 后默认用户常为 `root`，可设回普通用户：

```powershell
ubuntu2204 config --default-user 你的用户名
```

或写入 `/etc/wsl.conf`：

```powershell
wsl -d Ubuntu-22.04 -u root
```

```bash
echo -e "[user]\ndefault=你的用户名" | tee /etc/wsl.conf
exit
```

```powershell
wsl --shutdown
wsl -d Ubuntu-22.04
```

**⑤ 验证**

```powershell
wsl -l -v
```

确认对应发行版的 `VERSION` 为 `2`。若为 `1`：

```powershell
wsl --set-version Ubuntu-22.04 2
```

---

### 旧版手动离线流程（仅内核更新包时）

只有十几 MB 的 `wsl_update_x64.msi`、没有新版 WSL MSI 时，可走旧流程：

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
# 重启后
msiexec /i wsl_update_x64.msi
wsl --set-default-version 2
```

再安装 Ubuntu（`.wsl` / Store / `--import`）。  
能拿到 [GitHub Releases](https://github.com/microsoft/WSL/releases) 里的 `wsl.*.x64.msi` 时，优先用新本体，不必死磕 update 包。

---

### 迁移 WSL 安装位置

默认占用 C 盘时可迁到其他盘。

#### 新版（优先）

```powershell
wsl --shutdown
wsl -l -v
wsl --manage Ubuntu --move D:\WSL\Ubuntu
```

没有 `--manage` / `--move` 时先：`wsl --update`。

#### 导出再导入（兼容性更好）

```powershell
wsl --shutdown
wsl --export Ubuntu D:\WSL\ubuntu-backup.tar
wsl --unregister Ubuntu
wsl --import Ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu-backup.tar --version 2
```

然后按上文恢复默认用户。确认导入无误后再删除 `ubuntu-backup.tar`。

> `--unregister` 会删除该发行版原数据，务必先确认导出成功。

---

### 常用命令

| 操作 | 命令 |
|------|------|
| 进入默认发行版 | `wsl` |
| 列出发行版 | `wsl -l -v` |
| 关闭全部 WSL | `wsl --shutdown` |
| 指定发行版 | `wsl -d Ubuntu` |
| 更新 WSL | `wsl --update` |
| 卸载发行版 | `wsl --unregister Ubuntu` |

Windows 访问 Ubuntu 文件：资源管理器地址栏输入 `\\wsl$\Ubuntu\home\用户名`  
Ubuntu 访问 Windows：`/mnt/c/Users/你的用户名`

---

### apt 软件源（默认不是国内源）

官方 Ubuntu WSL 镜像的 apt 源一般指向 **国际源**（`archive.ubuntu.com` / `security.ubuntu.com`），**不会自动换成中国镜像**。国内访问慢或超时时，再手动换源。

#### 查看当前源

```bash
# Ubuntu 24.04 常见
cat /etc/apt/sources.list.d/ubuntu.sources

# 兼容查看
cat /etc/apt/sources.list
ls /etc/apt/sources.list.d/
```

若出现 `archive.ubuntu.com`、`security.ubuntu.com` → 国际源；  
若出现 `mirrors.aliyun.com`、`tuna.tsinghua.edu.cn`、`mirrors.ustc.edu.cn` 等 → 国内源。

#### Ubuntu 24.04（noble，推荐按这个改）

24.04 多用 DEB822 格式的 `ubuntu.sources`，不是老的单文件 `sources.list`。

```bash
sudo cp /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources.bak
sudo nano /etc/apt/sources.list.d/ubuntu.sources
```

可将内容改为清华镜像示例（代号 **noble**）：

```text
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
Suites: noble-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

保存后：

```bash
sudo apt update
```

一行替换（按实际 URI 是 http 还是 https 都覆盖）：

```bash
sudo sed -i 's|http://archive.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list.d/ubuntu.sources
sudo sed -i 's|http://security.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list.d/ubuntu.sources
sudo sed -i 's|https://archive.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list.d/ubuntu.sources
sudo sed -i 's|https://security.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list.d/ubuntu.sources
sudo apt update
```

#### Ubuntu 22.04（jammy）

22.04 常见为 `/etc/apt/sources.list`：

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i 's|http://archive.ubuntu.com|https://mirrors.tuna.tsinghua.edu.cn|g' /etc/apt/sources.list
sudo sed -i 's|http://security.ubuntu.com|https://mirrors.tuna.tsinghua.edu.cn|g' /etc/apt/sources.list
sudo apt update
```

#### 其他常用国内镜像

把上面地址里的域名换成即可：

| 镜像 | 地址前缀 |
|------|----------|
| 清华 TUNA | `https://mirrors.tuna.tsinghua.edu.cn/ubuntu/` |
| 阿里云 | `https://mirrors.aliyun.com/ubuntu/` |
| 中科大 | `https://mirrors.ustc.edu.cn/ubuntu/` |
| 华为云 | `https://mirrors.huaweicloud.com/ubuntu/` |

> 换源后务必 `sudo apt update`。若 `ubuntu.sources` 格式与示例不一致，先 `cat` 核对再改 `URIs` 行。

---

### 常见问题

#### `WSL_E_WSL_OPTIONAL_COMPONENT_REQUIRED`

报错「此应用程序需要适用于 Linux 的 Windows 子系统可选组件」时，说明 MSI 已装但可选组件未启用或未重启：

```powershell
wsl --install --no-distribution
```

然后**重启**，再执行 `wsl -l -v` / 安装 Ubuntu。

#### 提示需启用虚拟机平台 / 注册失败

- 确认 BIOS 虚拟化已开  
- 确认已启用「虚拟机平台」并重启  
- 尝试 `wsl --update`

#### 只有 wsl-update 十几 MB，装不起来

那是**内核更新包**，不是 WSL 本体。请到 GitHub Releases 下载 `wsl.*.x64.msi`，再到 releases.ubuntu.com 下载 `*-wsl-amd64.wsl`。

#### `wsl --install` 只显示帮助

说明组件已在，改用：

```powershell
wsl --install -d Ubuntu
```

#### 忘记 Linux 密码

```powershell
wsl -u root
passwd 你的用户名
```

---

### 参考链接

- [Install WSL - Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install)
- [WSL 本体 Releases](https://github.com/microsoft/WSL/releases)
- [Get Ubuntu on WSL](https://ubuntu.com/download/wsl)
- [Ubuntu 22.04 发布目录（含 wsl 镜像）](https://releases.ubuntu.com/22.04/)
- [Ubuntu 24.04 发布目录（含 wsl 镜像）](https://releases.ubuntu.com/24.04/)
- [清华 Ubuntu 镜像帮助](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
