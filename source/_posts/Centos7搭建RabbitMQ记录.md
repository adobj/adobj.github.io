---
title: Centos7搭建RabbitMQ记录
tags:
  - 消息队列
  - 分布式
abbrlink: 52fd
categories: uncategorized
date: 2020-01-05 17:26:48
---

闲来无事，打算学习一下大名鼎鼎的消息中间件，没想到安装过程中出现了不少问题，此处做个记录。

<!--more-->

## RabbitMQ简单介绍

RabbitMQ就是当前最主流的消息中间件之一。RabbitMQ是一个开源的AMQP实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗。在目前分布式的大环境下，成为非常常用的消息队列，以下详细说明怎么在centos7 上安装部署rabbitmq。

## 安装erlang

由于rabbitmq是基于erlang语言开发的，所以必须先安装erlang。

1)安装依赖
```
yum -y install gcc glibc-devel make ncurses-devel openssl-devel xmlto perl wget gtk2-devel binutils-devel
```
2)下载安装包，[官方链接](https://www.erlang.org/downloads)
```
wget http://erlang.org/download/otp_src_22.0.tar.gz
```
3)解压
```
tar -zxvf otp_src_22.0.tar.gz
```
4)安装
```
# 移动到指定目录
mv otp_src_22.0 /usr/local/
# 进入目录
cd /usr/local/otp_src_22.0/
# 创建安装目录
mkdir ../erlang
# 配置安装路径
./configure --prefix=/usr/local/erlang
# 安装
make install
# 查看是否安装成功
ll /usr/local/erlang/bin
# 添加环境变量
echo 'export PATH=$PATH:/usr/local/erlang/bin' >> /etc/profile
# 刷新环境变量
source /etc/profile
# 查看配置是否成功
erl
```
成功安装
![erlang](/images/rabbitmq/erlang.jpg)

## 安装RabbitMQ

下载安装包，[官方下载链接](https://github.com/rabbitmq/rabbitmq-server/tags)

此处为重中之重，一定要选择erlang对应版本的mq，我就是在此处掉坑，导致后边怎么折腾都无法启动mq。版本对应关系可以参考官网，此处给出[链接](https://www.rabbitmq.com/which-erlang.html)。
![rabbitmq](/images/rabbitmq/rabbitmq1.jpg)
```
# 下载
wget https://github.com/rabbitmq/rabbitmq-server/archive/v3.8.0.tar.gz
# 解压
tar -zxvf rabbitmq-server-3.8.0.tar.gz
# 移动
mv rabbitmq-server-3.8.0/ /usr/local/
# 配置环境变量
echo 'export PATH=$PATH:/usr/local/rabbitmq-server-3.8.0/sbin' >> /etc/profile
# 刷新环境变量
source /etc/profile
# 创建配置目录
mkdir /etc/rabbitmq
```
![rabbitmq](/images/rabbitmq/rabbitmq2.jpg)
## 基础操作
```
# 启动：
rabbitmq-server -detached
# 停止
rabbitmqctl stop
# 状态
rabbitmqctl status
# 开启web插件
rabbitmq-plugins enable rabbitmq_management
```
访问：http://127.0.0.1:15672/
![rabbitmq](/images/rabbitmq/rabbitmq3.jpg)
默认账号密码：guest guest（这个账号只允许本机访问）
```
# 查看所有用户
rabbitmqctl list_users
# 添加一个用户
rabbitmqctl add_user adobj 123456
# 配置权限
rabbitmqctl set_permissions -p "/" adobj ".*" ".*" ".*"
# 查看用户权限
rabbitmqctl list_user_permissions adobj
# 删除用户（安全起见，删除默认用户）
rabbitmqctl delete_user guest
```
配置好用户之后重启一下rabbit，然后就可以用新账号进行登陆
![rabbitmq](/images/rabbitmq/rabbitmq4.jpg)

