---
layout: post
title: 在DigitalOcean 安装 Shadowsocks，科学上网
tags: DigitalOcean Shadowsocks 科学上网
---

## 科学上网：用vps搭建shadowsocks

首先，非常感谢shadowsocks！

之前都是购买vpn，网络速度时好时坏，实在受不了，还是自己动手搭建ss吧。 

许多人都推荐shadowsocks来fq，今天我尝试搭建，并且记录下过程。

## vps 服务器设置

我之前有个digitalocean 优惠券，并且物美价廉，就用它了。

官网指导下，

- 安装Droplets, 选择 Ubuntu 14.04 x64
- 设置ssh，因为之前我配置过了，命令`cat ~/.ssh/id_rsa.pub` ， 添加ssh；初次设置，看教程：[点这里](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)
- Mac或linux登录shell, ssh root@ IP ADDRESS ，提示输入密码

## 安装Shadowsocks

进入vps 的shell后，执行命令

#### Ubuntu：
```
apt-get install python-pip
pip install shadowsocks
```
就这样，搞定安装了shadowsocks！

## 编写配置文件

用 vi 新建一个配置文件：

`vi /etc/shadowsocks.json`

然后输入以下内容：


```
{ 
   "server":"IP ADDRESS", 
   "server_port":25, 
   "local_address": "127.0.0.1", 
   "local_port":1080, 
   "password":"mypassword",
    "timeout":300, 
   "method":"aes-256-cfb", 
   "fast_open": false
}
```

需要修改IP ADDRESS，和 passwrod ， 保存退出。

## 启动 shadowsocks

后台启动和停止 shadowsocks 服务器：

```
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```

## 安装shadowsocks 客户端

shadowsocks 支持 windows、Mac OS X、Linux、Android、iOS 等多个平台。不过 iOS 由于系统对应用后台运行的限制，推荐使用客户端内嵌的浏览器科学上网，给其他应用代理时需要每过几分钟重新启动一下 app。

shadowsocks 项目 Github 主页在[这里](https://github.com/shadowsocks)。

里面可以找到客户端下载地址。

下载安装客户端以后，只需按服务器的配置填写 IP 地址、服务器端口、本地端口（如果没有本地端口选项，就是默认的 1080）、密码、加密方式等参数，启动就可以了。



## 加速优化 - 锐速


锐速是一款非常不错的TCP底层加速软件，可以非常方便快速地完成服务器网络的优化，配合 ShadowSocks 效果奇佳。目前锐速官方也出了永久免费版本，适用带宽20M、3000加速连接，个人使用是足够了。如果需要，先要在锐速官网注册个账户。

然后确定自己的内核是否在锐速的支持列表里，如果不在，请先更换内核，如果不确定，请使用 手动安装。[网址](http://my.serverspeeder.com/w.do?m=lsl)

确定自己的内核版本在支持列表里，我用的是 `Ubuntu 14.04 x64 vmlinuz-3.13.0-45-generic`， 在DigitalOcean设置更改内核版本， 使用以下命令快速安装了。 


```
wget http://my.serverspeeder.com/d/ls/serverSpeederInstaller.tar.gz
tar xzvf serverSpeederInstaller.tar.gz
bash serverSpeederInstaller.sh
```

输入在官网注册的账号密码进行安装，参数设置直接回车默认即可，
最后两项输入 y 开机自动启动锐速，y 立刻启动锐速。之后可以通过lsmod查看是否有appex模块在运行。

到这里还没结束，我们还要修改锐速的3个参数，vi /serverspeeder/etc/config


```
rsc="1" #RSC网卡驱动模式  
advinacc="1" #流量方向加速  
maxmode="1" #最大传输模式

```

digitalocean vps的网卡支持rsc和gso高级算法，所以可以开启rsc="1"，gso="1"。

重新启动锐速

`service serverSpeeder restart`


--------------------------

当我打开youtube，HD 高清1080p视频时， `飕`了一下，丝丝润滑，那种兴奋感觉，你懂的！

如果你需要  搭建的，点击这里，你我都有优惠赠送10美金。

OK!


> https://www.textarea.com/ExpectoPatronum/kexue-shangwang-yong-vps-dajian-shadowsocks-fuwuqi-265/
> http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/




