---
title: proxy-> SS+BBR 科学上网
date: 2019-04-15 22:06:31
tags: 
 - proxy
categories:
 - server
---

### 前言: 

因为github student pack有优惠码, 遂上DigitalOcean租了个服务器(送的100刀居然只有两个月有效期), 于是就顺便搭个梯子方便上网, 对了别租DO的新加披节点, 要连接它得跳到美国西部再到服务器, 延迟高网速慢. 上了San Francisco节点速度才上去, 但也很慢, 搜了下发现有个技术可以显著提速, 那就是BBR了(需要linux kernel>4.9).

### 安装SS, 用systemd启动

```bash
sudo apt update
sudo apt install shadowsocks-libev
```
有好几种, python go libev(binary) 等版本, 这里选这个纯粹图个方便, 可以自己选择, 不过本教程只做libev的, 其他版本也差不多可以去看ss官网.

```bash
sudo nano /etc/shadowsocks-libev/config.json
```
<!-- more -->
内容和下方类似, 自己修改一下.
```json
{
    "server":"sever_ipv4_address",
    "server_port":2333,   // 端口确保没被防火墙阻拦
    "local_port":1080,    // 这个不用管默认就好
    "password":"passwd",  // 自己起一个密码
    "timeout":60,
    "method":"rc4-md5"    // 还有chacha20之类的自己选择
}
```

```bash
systemctl start shadowsocks-libev.service

systemctl enable shadowsocks-libev.service
# 开启开机自启

systemctl status shadowsocks-libev.service
# 现在可以查看运行状态了

# systemctl restart shadowsocks-libev.service
# 重启ss服务, 用stop停止
```

可能出现的一些问题:
```bash
sudo apt install rng-tools
rngd -r /dev/urandom
# 如果ss显示不能快速产生随机数则运行上面两行

# 如果使用`systemctl`出现错误则使用这个
nohup ss-server -c /etc/shadowsocks-libev/config.json -u &
```

### 使用BBR

> **首先确定你的linux kernel版本要大于4.9**(使用 `uname -r` 命令)

如果内核版本不满足, 请自行搜索升级方法.

```bash
sysctl net.ipv4.tcp_available_congestion_control
# 返回值一般为：net.ipv4.tcp_available_congestion_control = bbr cubic reno

sysctl net.ipv4.tcp_congestion_control
# 返回值一般为：net.ipv4.tcp_congestion_control = bbr

sysctl net.core.default_qdisc
# 返回值一般为：net.core.default_qdisc = fq

lsmod | grep bbr
# 返回值有tcp_bbr则说明已经启动
```

之后不用任何操作, BBR已经在执行了.
另外还有什么锐速, kcptun之类的, 就我个人而言BBR已经提速非常明显了, 我也懒得搞kcptun了. 别人博客写的"kcptun 相当于外面包一层 UDP ，然后设置多倍发包，月经量飞速流逝", 就自行斟酌了吧.

可以参考[这个链接](https://zhuanlan.zhihu.com/p/29295440)更新kernel和开启bbr