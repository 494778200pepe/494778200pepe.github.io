---
layout: post
title:  "3 登录与授权、HTTPS 和 TCP/IP 协议族"
date:   2018-10-14 09:15:00 +0800
categories: HenCoder
tags: HenCoder_plus
author: pepe
description: 『 登录与授权、HTTPS 和 TCP/IP 协议族 』
---

HTTP中确认授权(或登录)的两种方式

* 1、通过 Cookie

* 2、通过 Authorization Header


### TCP/IP协议族

* Application Layer 应用层：HTTP、FTP、DNS 

* Transport Layer 传输层：TCP、UDP  (拆包，解决数据重传、分块，有的需要确认，有的不需要确认)

* Internet Layer 网络层：IP  (寻址、路由，传就好了)

* Link Layer 数据链路路层：以太网、Wi-Fi 


### HTTPS

* HTTP over SSL

* SSL：Secure Socket Layer -> TLS：Transport Layer Secure

* 定义：在HTTP之下增加的一个安全层，用于保障HTTP的加密传输。

* 本质：在客户端和服务端之间协商出一个对称秘钥，每次发送消息之前将内容加密，收到之后解密，达到内容的加密传输

* 为什么不直接用非对称加密？

### HTTPS连接

大致过程：

* 客户端请求建立TLS连接(通过TCP)

* 服务器发回证书(通过TCP)

* 客户端验证服务器证书(通过TCP)

* 客户端信任服务器后，和服务器协商对称秘钥(通过TCP)

* 使用对称秘钥开始通信 


详细过程：

* 1. (TCP)Client Hello (客户端能接受的TLS版本(1.0、2.0等等)、Cipher Suite(对称加密算法(多个)，非对称加密算法(多个)，Hash算法(多个))，还有一个客户端随机数)
* 2. (TCP)Server Hello (服务端能选择的TLS版本(1.0)、Cipher Suite(对称加密算法，非对称加密算法，Hash算法，三个算法打包一起选择)，还有一个服务端随机数)
* 3. (TCP)服务器证书 信任建立 (附加：服务器的地址，证书公钥，证书签名以及证书机构的公钥和其他信息)
* 4. Pre-master Secret (用服务器证书公钥，非对称机密，得到一个Pre-master Secret，发送给服务端)[两个随机数，再加上Pre-master Secret得到一个Master Secret]
* 5. 客户端通知：将使用加密通信 
* 6. 客户端发送：Finished 
* 7. 服务器通知：将使用加密通信 
* 8. 服务器发送：Finished 


参考：

[TCP的三次握手与四次挥手（详解+动图） - qzcsu的博客 - CSDN博客](https://blog.csdn.net/qzcsu/article/details/72861891)