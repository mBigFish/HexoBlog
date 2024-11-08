---
title: kali连接蓝牙
date: 2024-09-02 10:42:01
cover: ../../images/kali连接蓝牙/image-20240902104124699083.png
excerpt: kali 连接蓝牙 一、启动蓝牙服务 Kali Linux 默认状态下并没有启动蓝牙服务，如果尝试打开蓝牙管理器会提示 Bluez 守护进程没有运行，不能继续。应用下列指令开启蓝牙服务： sudo service bluetooth start 二、配对设备 开启蓝牙服务后右上角状态栏出现了蓝牙图
permalink: /archives/1725244920854/
categories:
 - [linux]
 - [问题解决]
tags: 
 - 归纳
---

# kali 连接蓝牙

## 一、启动蓝牙服务

* **Kali Linux 默认状态下并没有启动蓝牙服务，如果尝试打开蓝牙管理器会提示 Bluez 守护进程没有运行，不能继续。应用下列指令开启蓝牙服务：**

```
sudo service bluetooth start
```

## 二、配对设备

* **开启蓝牙服务后右上角状态栏出现了蓝牙图标。**
* **点击蓝牙图标并选择“设置新设备“。**
* **此时配对目标蓝牙设备，这时不会出现问题。**

## 三、连接设备

* **此时可能会出现“Device successfully added.But failed to connect.”**
* **这个问题并不是蓝牙协议或者其他原因造成的。**
* **造成它的原因仅仅是 Blueman-manager 本身。**

## 四、问题解决

* **这时候我们只需要换一种方式连接蓝牙设备就可以了。**
* **打开 Terminal，输入：**

```
sudo bluetoothctl
```

* **此时会进入蓝牙管理进程。**
* **再点击右上角的蓝牙图标，选择“设备”。**

![img](../images/kali连接蓝牙/image-20240902104124699083.png)

* **右键已经配对的设备，选择“信息”，并复制当前设备的 MAC 地址。**
* **返回 Terminal，输入：**

```
trust MAC 地址（这里粘帖刚刚复制好的 MAC 地址）
connect MAC 地址（这里一样是刚刚复制好的 MAC 地址）
```

![img](../images/kali连接蓝牙/image-20240902104130308673.png)

## 五、收尾

* **此时系统并不会把默认设备自动切换到刚刚连接的蓝牙设备。**
* **以我刚刚连接的蓝牙耳机为例：**
* **点击音频控制按钮，并在下拉栏中将输出设备替换为蓝牙耳机。**

![img](../images/kali连接蓝牙/image-20240902104133770369.png)

### 参考

* [https://www.cnblogs.com/bobdylan/p/6933784.html](https://www.cnblogs.com/bobdylan/p/6933784.html)
* [https://codeantenna.com/a/TMOMv1BsA3](https://codeantenna.com/a/TMOMv1BsA3)
