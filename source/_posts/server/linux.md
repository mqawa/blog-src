---
id: 21122801
title: Linux 常用命令
date: 2021-12-28 09:29:00
description: Linux 使用过程中的一些命令（常用，但不完全常用）
tags: server
categories: 
  - [学习笔记, server]
cover: https://blog-1301412627.cos.ap-beijing.myqcloud.com/blog-res/images/server/linux/linux.jpg
---
***
## 查看端口占用并解除
```bash
# 语法: lsof -i[46] [protocol][@hostname|hostaddr][:service|port]
# 查看 8000 端口占用情况
lsof -i:8000
# 解除占用
# kill [pid] | kill -KILL [pid] 强制解除
kill -KILL 28939
```
***
## 后台运行 解放终端
### nohup
```bash
# command 为需要运行的命令
nohup command &
# 常用命令可以写一个脚本
```
***
### screen
```bash
# screen 适用于需要持续运行且随时需要操作的命令 列如 code-server

# 开启一个新的进程
screen
# 运行命令
command
# 让当前窗口进入后台
Ctrl + A + D
# 查看所有后台进程
screen -ls
# 进入后台进程
screen -r id
# 关闭进程
exit | Ctrl + C
# 强制关闭
screen -S id -X quit
```
***