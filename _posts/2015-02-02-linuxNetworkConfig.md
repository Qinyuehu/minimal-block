---
layout: post
title: "Some commands for Linux network configration"
date: 2015-02-02 pm
categories: Linux
---
*ip address
*network mask
*route
*DNS

测试网络连通性
 ping 192.168.1.1
 ping www.hostname.net

测试DNS resolv
 host www.hostname.com
 dig www.hostname.net

显示路由表
 ip route

追踪到达目标地址的网络路径
 traceroute www.hostname.com

使用mtr进行网络质量测试（结合了traceroute和ping)
 mtr www.hostname.com
