---
title: what-happend
date: 2019-04-16 15:18:28
tags:
categories:
---

## 发生了什么?

**请完整的阅读本文**

你收到了一份礼物, 包含一台云服务器. 我通过微信已经把ip地址和登录密码发给了你, 默认用户是 `root` , 可以使用ssh登录服务器, 然后搭建你想搭的任何东西.

### 为什么送服务器?

1. DigitalOcean因为教育账号送了我100美刀(但有效期只有两个月...)
2. 帮助有能力的同学了解更多, 学习更多.
3. 不能便宜了DO...(DigitalOcean的简称)
<!-- more -->

### **关于服务器(重要)**

1. 服务器搭建在digitalocean的旧金山数据中心(中国访问最快的节点)
2. 默认给大家搭建的 `ubuntu 18.10` 版本.![](/images/inpost/what-happend.jpg)
3. 由于每个人的需求可能不同, 我没在DO上设置防火墙阻隔端口, 有需要的自行使用ufw或iptable之类的服务器端防火墙.
4. 不要公开自己的ip地址(当然还有ssh密码)到网络上(被ddos我会直接关服的)
5. **请低调使用**, 因为资源不是很充足, 所以没给班上所有人都来一个, 而且不是所有人都有能力用上, 空开着个服务器没人用太浪费了. 
**如果用不上请私聊我, 我会收回服务器.**
6. **服务器仅支持2个月**, 这些服务器都是最便宜的版本(1G RAM 25G SSD 流量每月1TB), 这样每月要5刀(折合人民币约35元), 我是靠赠币买的, 到期我会提前通知然后关闭服务器.

## 如何使用呢?

可以使用ssh远程登录服务器, 可以使用xshell之类, 或者powershell好像有自带ssh(打开输入ssh看有没有反应). 这个就自行斟酌了.
```bash
user@COMPUTER:~$ ssh root@68.183.xxx.xxx
root@68.183.xxx.xxx''s password: #这里输入了不会显示
You are required to change your password immediately (root enforced)
Welcome to Ubuntu 18.10 (GNU/Linux 4.18.0-10-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Apr 16 08:31:08 UTC 2019

  System load:  0.0               Processes:           86
  Usage of /:   4.5% of 24.06GB   Users logged in:     0
  Memory usage: 14%               IP address for ens3: 68.183.xxx.xxx
  Swap usage:   0%

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


Last login: Tue Apr 16 08:24:07 2019 from xxx.xxx.xxx.xxx
Changing password for root.
(current) UNIX password:
```
使用ssh登陆后会让你重置密码, 然后需要你自行设置机器密码, 之后登录就用这个而不是我发你的初始随机密码

## 我能用它来干什么?

1. 搭建网站, 你所看到的这个网站就是搭建在我的那台云服务器上. 申请个域名, 建好站, 申请个免费`ssl`证书(enable https), 你也能拥有像我这样的网站
2. 搭建梯子, 因为是美国的服务器, 所以可以拿科学上网. [搭建教程](/posts/proxy)
3. 建一个mysql数据库, 将来梗爷的课设上可以用到?
4. 做一个twitterbot, 定时每天早上7:30给你推送天气和空气信息.
5. 等等等等, 可以去知乎搜下vps可以做些什么.


## 其他问题

Q: 我需要付费么?
A: *不需要支付任何开销*, 这台vps就是送给你玩2个月的.
   **你只需要尽可能的从中学到更多**

Q: 到期了我的服务器会怎样? 数据呢?
A: 我会关闭服务器, **数据需要你自己备份**. 我会在关闭前通知你.

Q: 我想搭一个像你这样的网站.
A: 在网页底部有链接, Driven - `Hexo` Theme - `Melody`. 可以点进去看官方文档.

Q: 你的`SSL证书`是免费的? 我也想要一个.
A: 是免费的, 请搜索 `Let's Encrypt`. 它提供了一个 `CertBot` 可以给你的主机颁发并启用`SSL证书`
