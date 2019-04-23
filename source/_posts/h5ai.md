---
title: h5ai & apache添加子域名
date: 2019-04-17 22:29:21
tags: 
 - h5ai
 - apache
categories: 服务器
---

## 添加subdomain

首先要去域名提供商添加域名的一个新的A类record, 如:
```
Type  Hostname          Address       TTL(seconds)
 A   h5ai.HINATAA.TK  142.xxx.xxx.xx  1800(default)

# h5ai是输入的, 后面的主域名提供商会自己识别(不然你指向google.com怎么办), 不需要自己输入.
```
*生效需要一定的时间*, 期间还是会显示`找不到 h5ai.hinataa.tk 的服务器 IP 地址`.

## h5ai 
> https://larsjung.de/h5ai/ # 项目主页

> https://larsjung.de/h5ai/demo/ # 实现demo
<!-- more -->
### 保存h5ai文件
先下载下来, 解压能得到一个 `_h5ai` 文件夹,
```bash
curl -o h5ai.zip https://release.larsjung.de/h5ai/h5ai-0.29.2.zip
# 或者
wget https://release.larsjung.de/h5ai/h5ai-0.29.2.zip
# 自行替换链接为最新release的链接

mv _h5ai /var/www/ftp/
# 随便放哪, 后面指定apache配置的`DocumentRoot`更改一下目录就行
```

### apache2配置
```bash
nano /etc/apache2/sites-available/h5ai.hinataa.tk.conf
# 添加配置文件
```
```conf
<VirtualHost *:80>
        ServerName h5ai.hinataa.tk
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/ftp

        #ErrorLog ${APACHE_LOG_DIR}/error.log
        #CustomLog ${APACHE_LOG_DIR}/access.log combined
        
        DirectoryIndex /_h5ai/public/index.php
        <Directory "/var/www/ftp">
            AllowOverride all
            Order allow,deny
            Allow from all
        </Directory>
</VirtualHost>
```
**注意! 务必添加`/_h5ai/public/index.php`到默认index队列**
例如此处的配置文件需要设置`DirectoryIndex /_h5ai/public/index.php`, 否则`apache`找不到你的index位置.

[original link](https://haofly.net/apache/index.html)

```bash
ln -s /etc/apache2/sites-available/h5ai.hinataa.tk.conf /etc/apache2/sites-enabled/h5ai.hinataa.tk
a2ensite h5ai.hinataa.tk
service apache2 restart
```

## 等待全球dns服务器刷新...
如果你等了一天还没用 (找不到网页的ip地址), 那你可能需要给你的域名使用一个别的`DNS NameServer`(比如`cloudflare`).

如果你的子域名一直被重定向到主域名所在的文件目录(比`h5ai`这个子域名显示的内容和`www`的一样), 则你需要看一下这个教程->[link](https://haofly.net/apache/index.html).