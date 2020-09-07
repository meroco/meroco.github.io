---
title: HTTPS+HTTP/2.0
date: 2016-08-14 13:47:55
categories: 
      - 其它
---
给博客开启了 **HTTPS** 和 **HTTP/2.0**，解决 HTTP 协议安全问题的同时让访问速度进一步提升。

<!-- more -->

 **SSLLABS** 的评分测试，配置上赫尔曼密钥或许能变成 A+ 吧，然而懒癌发作。

![ssllabs.png](https://i.loli.net/2020/02/20/K1CxiPSeqsjM8u9.png)

配置好 HTTPS 后顺便升级了 **NGINX** 版本 1.11.1，**OPENSSL** 版本 1.0.2h，这样就能使用 **HTTP/2.0** 了。

![http2.png](https://i.loli.net/2020/02/20/PYhDbISsRN6fz2k.png)

目前使用的是 STARTSSL 的免费证书，期待今年圣诞节一波打折活动，准备买三年期授权的 COMODO Positive SSL。
