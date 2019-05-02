---
title: webdav
date: 2019-05-02 19:11:13
tags: 
 - apache
 - webdav
categories: server
---

## webdav

先添加`dav`的subdomain, 见[这篇文章](/posts/h5ai/).

## server side

新建用户:
```bash
sudo adduser davuser # 用户名自己起

mkdir /var/www/webdav
```

开启`apache`的`webdav mod`.
```bash
sudo a2enmod dav
sudo a2enmod dav_fs
nano /etc/apache2/sites-available/dav.hinataa.tk.conf
```
```json
DavLockDB /var/www/DavLock
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/webdav

        <Directory "/var/www/webdav">
            DAV ON
        </Directory>
</VirtualHost>
```

```bash
ln -s /etc/apache2/sites-available/dav.hinataa.tk.conf /etc/apache2/sites-enabled/dav.hinataa.tk
a2ensite dav.hinataa.tk
service apache2 restart
```

## 增加密码认证
```bash
apt-get install apache2-utils
htpasswd -c /etc/apache2/webdav.passwords davuser
# 给davuser创建一个密码

chown www-data:www-data /etc/apache2/webdav.passwords
# 更改文件权限给apache让他可以读写

nano /etc/apache2/sites-available/dav.hinataa.tk.conf
```

在`<Directory>`session添加下列代码
```
AuthType Basic 
AuthName "webdav" 
AuthUserFile /etc/apache2/webdav.passwords 
Require valid-user
```
> 完全版本:
```
DavLockDB /var/www/DavLock
<VirtualHost *:80>
        ServerName dav.hinataa.tk
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/webdav
        <Directory "/var/www/webdav">
            DAV ON
            AuthType Basic 
            AuthName "webdav" 
            AuthUserFile /etc/apache2/webdav.passwords 
            Require valid-user
        </Directory>
</VirtualHost>
```

```bash
a2enmod auth_basic
service apache2 restart
```

## 映射磁盘

现在可以在windows文件管理器里映射磁盘了, 右键点击网络, 选择`"映射网络驱动器(N)..."`
![](/images/inpost/webdav-1.png)
![](/images/inpost/webdav-2.png)

然后就能挂载远程webdav作为一个windows磁盘, 能在命令行切进目录去, 能直接打开修改保存文件, 对于日常维护的文本文件读写速度完全能够满足.