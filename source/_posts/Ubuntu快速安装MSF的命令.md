---
title: Ubuntu快速安装MSF的命令
date: 2024-09-02 21:38:24
categories:
 - [linux]
 - [搭建安装]
tags: 
 - 搬砖
---

# Ubuntu快速安装MSF的命令

#### 1、这个官方安装脚本，我们只需要运行一下就行了

```
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \chmod 755 msfinstall && \./msfinstall
```

#### 2、安装完成后，msf 使用的是 postgresql 数据库 , 如果没有的话需要先安装

```
sudo apt install postgresql
```

#### 3、安装完成之后确认下服务是否开启，如果没有开启它

```
sudo service postgresql status
```

#### 4、接着输入 `msfconsole` 会提示你是否建立一个数据库，你输入 y 就好

#### 5、测试一下数据库是否可以使用

```
search windows
```

#### 6、如果还是不可以的话，可以进入 metasploit 中，输入 y 就好

```
 msfdb init
```

#### 7、msf 安装好的目录位于

```
/opt/metasploit-framework
```

#### 8、msf 更新命令：

```
sudo msfupdate
```

**搬砖：**[https://www.cnblogs.com/tomyyyyy/p/12813299.html](https://www.cnblogs.com/tomyyyyy/p/12813299.html)
