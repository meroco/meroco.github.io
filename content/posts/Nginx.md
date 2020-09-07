---
title: 关于 NGINX 的二三事
date: 2016-10-16 13:12:05
cover: https://i.loli.net/2020/02/20/n3UcX6Nv81KIlV7.jpg
categories: 
     - 其它
---

# NGINX 简介

> [{% ruby Nginx|engine x %}](https://nginx.org/en/) 是一个网页服务器，它能反向代理 `HTTP`, `HTTPS`, `SMTP`, `POP3`, `IMAP` 的协议链接，以及一个负载均衡器和一个 `HTTP` 缓存。
起初是供俄国大型的门户网站及搜索引擎 [{% ruby Rambler|Рамблер %}](https://www.rambler.ru) 使用。此软件 [BSD-like](https://zh.wikipedia.org/wiki/BSD许可证) 协议下发行，可以在 `UNIX`、`GNU/Linux`、`BSD`、`Mac OS X`、`Solaris`，以及 `Microsoft Windows` 等操作系统中运行。

以上是维基百科对 Nginx 的描述。

<!-- more -->

***

# 编译安装
编译安装区别于 `yum` 安装，最大的优点就是能随心所欲，缺点是过程略微复杂。
## 环境和目标

|[![](https://img.shields.io/badge/Aliyun-ECS-orange.svg)](https://www.aliyun.com/product/ecs)|[![](https://img.shields.io/badge/CentOS-7.2-yellow.svg)](https://www.centos.org)|[![](https://img.shields.io/badge/NGINX-1.11.1-brightgreen.svg)](https://nginx.org/en/)|[![](https://img.shields.io/badge/OpenSSL-1.0.2h-blue.svg)](https://www.openssl.org)|
|:-:|:-:|:-:|:-:|

## 安装编译工具和依赖包
``` sh
$ yum -y install gcc gcc-c++ autoconf automake
$ yum -y install zlib zlib-devel openssl openssl-devel pcre-devel
```
如果报错搜索错误代码解决。
##  新建匿名用户和用户组
``` sh
$ sudo groupadd -r nginx
$ sudo useradd -s /sbin/nologin -g nginx -r nginx
```
## 下载源码
``` sh
$ cd /root
$ wget http://nginx.org/download/nginx-1.11.1.tar.gz
$ wget https://www.openssl.org/source/openssl-1.0.2h.tar.gz
```
建议去官网查看下载当前最新的稳定版本。下载新版 [OpenSSL](https://www.openssl.org) 的目的是为了支持 `HTTP/2`。 
## 解压并编译安装
### 解压文件
``` sh
$ tar -zxcf nginx-1.11.1.tar.gz
$ mv openssl-1.0.2h.tar.gz /root/nginx-1.11.1
$ cd /root/nginx-1.11.1
$ tar -zxcf openssl-1.0.2h.tar.gz
```
### 配置编译参数
``` sh
./configure --user=nginx --group=nginx --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-ipv6 --with-http_sub_module --with-openssl=openssl-1.0.2h
```
这是笔者的编译参数，可以通过命令 `./configure --help` 查看全部参数说明，然后根据需要选择。
### 编译安装
``` sh
$ make && make install
```
如果一切顺利很快就能完成安装。 

***

# 常用命令
## 启动
``` sh
$ cd /usr/local/nginx/sbin
$ ./nginx
```
## 重启
``` sh
$ cd /usr/local/nginx/sbin
$ ./nginx -s reload
```
## 配置文件校验
``` sh
$ cd /usr/local/nginx/sbin
$ ./nginx -t
```
## 关闭
### 查询主进程号
``` sh
$ ps -ef | grep nginx
```
### 从容停止
``` sh
$ kill -QUIT 主进程号
```
### 快速停止
``` sh
$ kill -TERM 主进程号
```
### 强制停止
``` sh
$ kill -9 nginx
```
### PID 停止
``` sh
$ kill -信号类型 ‘/usr/local/nginx/logs/nginx.pid’
```
### 解决 PID 丢失
``` sh
$ /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```
## 版本信息
``` sh
$ cd /usr/local/nginx/sbin
$ ./nginx -V
```
## 升级
同安装基本类似，下载、解压、编译、升级。

``` sh
$ make upgrade
```

***

# 配置文件参数
`nginx.conf` 文件的配置参数。以笔者的配置参数作为参考，主要做了 `HTTP` 强制跳转 `HTTPS` 并开启了 `HTTP/2` 两个功能。

``` sh
server {
        listen 80;
        listen [::]:80;

        return 301 https://$host$request_uri;
    }
server
    {
        listen  443 ssl http2;
        listen [::]:443 ssl http2;
        server_name misuzu.moe www.misuzu.moe blog.misuzu.moe;
        index index.html index.htm index.php;
        root  /home/www/blog;
        
        ssl_certificate /home/www/ssl/misuzu.crt;
        ssl_certificate_key /home/www/ssl/misuzu.key;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_session_cache shared:SSL:20m;
        ssl_session_timeout 60m;
    }
```

***

# 结语
> 好记性不如烂笔头。

个人笔记。
