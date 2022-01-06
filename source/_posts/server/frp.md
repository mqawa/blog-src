---
id: 21122002
title: frp 使用教程
date: 2021-12-20 15:10:00
description: frp - 一款优秀开源免费的内网穿透工具
tags: server
categories: 
  - [学习笔记, server]
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/frp.png
---
***
## 简介

**frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。**

**frp 项目开源地址 : [https://github.com/fatedier/frp](https://github.com/fatedier/frp)**
**本文资源下载地址 :**
+ **[Cloudreve](https://cloud.mqawa.com/s/8efq)**
+ **[百度网盘](https://pan.baidu.com/s/1AERgN1Mr1gb7gwvuzsfYMw)，提取码：niit**

***

## frp 有什么用

+ 使用个人电脑搭建服务器
+ 远程控制个人电脑
+ 端口流量转发
+ 数据代理

***

## 准备工作
+ **一台具有公网 ip 的服务器**
+ **ssh 工具**

***

## 服务器安装配置 frp

***

### Linux 服务端
#### 查看 Linux 系统架构
```bash
uname -a
```
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/linux.png)
#### 下载对应版本 frp
```bash
wget https://github.com/fatedier/frp/releases/download/v0.38.0/frp_0.38.0_linux_amd64.tar.gz
```
#### 解压并移动到 /usr/local 目录
```bash
tar zxvf frp_0.38.0_linux_amd64.tar.gz
mv frp_0.38.0_linux_amd64/ /usr/local/frp
```
#### 编辑配置文件
```bash
cd /usr/local/frp
vim frps.ini
```
**按 i 输入**
```bash
[common]
bind_port = 7000           #与客户端绑定的进行通信的端口
```

**按 esc 输入:wq 回车保存退出**
```bash
#为 frps 赋予运行权限
chmod +x frps 
./frps -c ./frps.ini
```
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/linux_start_success.png)
**启动成功**
#### 配置 frp 后台运行并开机启动
**按 Ctrl + C 中止 frp**
```bash
vim /etc/systemd/system/frp.service
```

**按 i 输入 粘贴下方代码**
```bash
[Unit]
Description=Frp Server
After=network.target
Wants=network.target

[Service]
Restart=on-failure
RestartSec=5
ExecStart=/usr/local/frp/frps -c /usr/local/frp/frps.ini

[Install]
WantedBy=multi-user.target
```

**按 esc 输入:wq 回车保存退出**
```bash
# 重载 systemctl
systemctl daemon-reload
# 启动 frp
systemctl start frp 
# 查看 frp 状态
systemctl status frp 
# 设置开机启动
systemctl enable frp
```
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/status_success.png)
**启动成功**

***

### Windows 服务端
#### 下载 frp 并解压
**win 报毒请忽略**
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/win_frp.png)
#### 配置 frp 服务端
**打开 frps.ini, 输入以下内容**
```
[common]
bind_port = 7000            #与客户端绑定的进行通信的端口
```

**保存退出**
**在地址栏输入 cmd 回车打开**

```
frps.exe -c frps.ini
```

**防火墙请放行**
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/win_success.png)
**运行成功**
***
## 客户端配置
***
### Linux 客户端
#### 查看 Linux 系统架构

```bash
uname -a
```

![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/linux.png)
#### 下载对应版本 frp

```bash
wget https://github.com/fatedier/frp/releases/download/v0.38.0/frp_0.38.0_linux_amd64.tar.gz
```

#### 解压并移动到 /usr/local 目录

```bash
tar zxvf frp_0.38.0_linux_amd64.tar.gz
mv frp_0.38.0_linux_amd64/ /usr/local/frp
```

#### 编辑配置文件

```bash
cd /usr/local/frp
vim frpc.ini
```

**按 i 输入**

```bash
[common]
server_addr = xxx.xxx.xxx.xxx
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
```

**配置文件详解**

```bash
[common]
server_addr = xxx.xxx.xxx.xxx   #公网服务器 ip
server_port = 7000            #与服务端bind_port一致

[ssh]                   #连接名称
type = tcp              #连接协议
local_ip = 127.0.0.1    #内网服务器ip
local_port = 22         #本地要公开的端口
remote_port = 6000      #服务器映射端口，可与本地端口不一致
```

**按 esc 输入:wq 回车保存退出**

```bash
#为 frps 赋予运行权限
chmod +x frpc
./frpc -c ./frpc.ini
```

![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/c_linux_start_success.png)
**启动成功**
#### 配置 frp 后台运行并开机启动
**按 Ctrl + C 中止 frp**

```bash
vim /etc/systemd/system/frp.service
```

**按 i 输入 粘贴下方代码**

```bash
[Unit]
Description=Frp Client
After=network.target
Wants=network.target

[Service]
Restart=on-failure
RestartSec=5
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.ini

[Install]
WantedBy=multi-user.target
```

**按 esc 输入:wq 回车保存退出**

```bash
# 重载 systemctl
systemctl daemon-reload
# 启动 frp
systemctl start frp
# 查看 frp 状态
systemctl status frp
# 设置开机启动
systemctl enable frp
```

![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/c_status_success.png)
**启动成功**
***
### Windows 客户端
#### 下载 frp 并解压
**win 报毒请忽略**
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/win_frp.png)
#### 配置 frp 客户端
**打开 frpc.ini, 输入以下内容**

```bash
[common]
server_addr = xxx.xxx.xxx.xxx
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 3389
```

**配置文件详解**

```bash
[common]
server_addr = xxx.xxx.xxx.xxx   #公网服务器 ip
server_port = 7000            #与服务端bind_port一致

[rdp]                   #连接名称
type = tcp              #连接协议
local_ip = 127.0.0.1    #内网服务器ip
local_port = 3389       #本地要公开的端口
remote_port = 3389      #服务器映射端口，可与本地端口不一致
```

**保存退出**
**在地址栏输入 cmd 回车打开**

```bash
# 运行frp 客户端
frpc.exe -c frpc.ini
```

**防火墙请放行**
![](https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/frp/c_win_success.png)
**运行成功**
***
## 一些问题
+ frp 客户端可在一个文件内同时配置多个连接
+ 客户端的一个端口可以被多个 frp 客户端监听
+ 服务端的一个端口只能被一个 frp 服务端监听
+ frp 支持 TCP、UDP、HTTP、HTTPS 等多种协议
+ Linux 报错请确认下载版本正确且赋予 frp 运行权限
+ Windows Defender 会把 frpc.exe 识别为病毒并隔离，请手动恢复
***
## 结束
**现在，就可以在您的 公网ip:端口 访问您内网机器的服务了**
**有问题请在评论区留言，博主将会尽快处理**
***