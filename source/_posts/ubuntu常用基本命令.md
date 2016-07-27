---
title: ubuntu常用基本命令
date: 2016-06-15 14:05:39
tags:
  - 我的Blog
  - ubuntu
categories:
  - 日志
  - 笔记
---

## 创建桌面图标
{%codeblock%}
gnome-desktop-item-edit /usr/share/applications --create-new 
{%endcodeblock%}

## ﻿安装 .deb文件
{%codeblock%}
  $ sudo passwd root 修改root密码
	$ sudo dpkg -i package_file.deb
{%endcodeblock%}

## 安装最新的 git
{%codeblock%}
  $ sudo add-apt-repository ppa:git-core/ppa //安装git 的源
  $ sudo apt-get update
  $ sudo apt-get install git
{%endcodeblock%}

## 安装 nodejs版本管理工具
{%codeblock%}
  $ sudo wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
	$ source ~/.bashrc
{%endcodeblock%}

## 安装mysql
{%codeblock%}
  $ sudo apt-get install mysql-server
{%endcodeblock%}
修改mysql配置:
  1. /etc/mysql/mysql.conf.d/mysqld.conf 添加  character-set-server=utf8 ，将bind-address=0.0.0.0
  2. /etc/mysql/conf.d/mysql.conf 添加  default-sourcharacter-set=utf8

## 查看系统进程操作
{%codeblock%}
  $ ps -aux|grep <进程名称>
  $ netstat -nlt|grep <端口号> //查看端口状态
  $ kill -9 <进程号> // 关掉进程
{%endcodeblock%}
