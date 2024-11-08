---
title: Linux安装v2ray
date: 2024-09-02 10:33:51
cover: 
excerpt: Linux安装v2ray 一、下载与安装 v2rayA 的功能依赖于 V2Ray 内核，因此需要安装内核。 （一）安装 V2Ray 内核 / Xray 内核 1.V2Ray / Xray 的官方脚本 V2Ray 安装参考：https//github.com/v2fly/fhs-install-v2
permalink: /archives/1725244431165/
categories:
 - [linux]
 - [工具使用]
tags: 
 - 笔记
---

# Linux安装v2ray

## 一、下载与安装

**v2rayA 的功能依赖于 V2Ray 内核，因此需要安装内核。**

### （一）安装 V2Ray 内核 / Xray 内核

#### 1.V2Ray / Xray 的官方脚本

* **V2Ray 安装参考：**[https://github.com/v2fly/fhs-install-v2ray](https://github.com/v2fly/fhs-install-v2ray)
* **Xray 安装参考：**[https://github.com/XTLS/Xray-install](https://github.com/XTLS/Xray-install)

#### 2.v2rayA 提供的镜像脚本（推荐）

```
sduo curl -Ls https://mirrors.v2raya.org/go.sh | sudo bash
```

* **安装后可以关掉服务，因为 v2rayA 不依赖于该 systemd 服务。**

```
sudo systemctl disable v2ray --now ### Xray 需要替换服务为 xray
```

### （二）安装 v2rayA

#### 1. 通过软件源安装

##### a. 添加公钥

```
wget -qO - https://apt.v2raya.org/key/public-key.asc | sudo tee /etc/apt/trusted.gpg.d/v2raya.asc
```

##### b. 添加 V2RayA 软件源

```
echo "deb https://apt.v2raya.org/ v2raya main" | sudo tee /etc/apt/sources.list.d/v2raya.list
sudo apt update
```

##### c. 安装 V2RayA

```
sudo apt install v2raya
```

#### 2. 手动安装 deb 包

##### a. 下载 deb 包

**下载后可以使用 Gdebi、QApt 等图形化工具来安装，也可以使用命令行：**

```
sudo apt install /path/download/installer_debian_xxx_vxxx.deb ### 自行替换 deb 包所在的实际路径
```

## 二、使用方法

### （一）启动 v2rayA / 设置 v2rayA 自动启动

* **从 1.5 版开始将不再默认为用户启动 v2rayA 及设置开机自动。**

#### 1. 启动 v2rayA

```
sudo systemctl start v2raya.service
```

#### 2. 设置开机自动启动

```
sudo systemctl enable v2raya.service
```

#### 3. 切换 iptables 为 iptables-nft

* **对于 Debian11 用户来说，iptables 已被弃用。使用 nftables 作为 iptables 的后端以进行适配：**

```
update-alternatives --set iptables /usr/sbin/iptables-nft
update-alternatives --set ip6tables /usr/sbin/ip6tables-nft
update-alternatives --set arptables /usr/sbin/arptables-nft
update-alternatives --set ebtables /usr/sbin/ebtables-nft
```

* **如果你想切换回 legacy 版本：**

```
update-alternatives --set iptables /usr/sbin/iptables-legacy
update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
update-alternatives --set arptables /usr/sbin/arptables-legacy
update-alternatives --set ebtables /usr/sbin/ebtables-legacy
```

* **切换后重启即可。**

### （二）创建帐号

* 如果你通过 2017 端口 如 [http://localhost:2017](http://localhost:2017/) 无法访问 UI 界面，请检查你的服务是否已经启动。相关问题接下来进入 UI，本节将介绍 v2rayA 的基本操作流程。
* 在第一次进入页面时，你需要创建一个管理员账号，请妥善保管你的用户名密码，如果遗忘，使用 sudo v2raya –reset-password 命令重置

### （三）导入节点

* **可以导入节点，也可以导入订阅节点。**
