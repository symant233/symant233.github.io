---
title: h5ai与nginx 添加子域名
date: 2019-04-17 22:29:21
tags: 
 - h5ai
 - nginx
categories: 服务器
---

## 添加subdomain

首先要去域名提供商添加域名的一个新的A类record, 如:
```
Type  Hostname          Address       TTL(seconds)
 A   h5ai.HINATAA.TK  142.xxx.xxx.xx  1800(default)

# h5ai是输入的, 后面的主域名提供商会自己识别(不然你指向google.com怎么办), 不需要自己输入.
```
*生效需要至少一个小时的时间*, 期间还是会显示`找不到 h5ai.hinataa.tk 的服务器 IP 地址`
还可以顺便添加一下以后会用上的subdomain...

## h5ai 
项目主页
> https://larsjung.de/h5ai/

实现demo
> https://larsjung.de/h5ai/demo/
<!-- more -->
### 保存h5ai文件
先下载下来, 解压能得到一个 `_h5ai` 文件夹,
```bash
curl -o h5ai.zip https://release.larsjung.de/h5ai/h5ai-0.29.2.zip
# 或者
wget https://release.larsjung.de/h5ai/h5ai-0.29.2.zip
# 自行替换链接为最新release

mv _h5ai /var/www/h5ai/
# 随便放哪, 后面指定nginx配置的`root`更改一下目录就行
nano /etc/nginx/sites-available/h5ai.hinataa.tk
# 现在该目录下有 default hinataa.tk h5ai.hinataa.tk文件
```

### nginx配置
```
server {
        listen 80;

        root /var/www/h5ai.hinataa.tk;
        index index.html index.htm /_h5ai/public/index.php;

        server_name h5ai.hinataa.tk;

        location / {
                try_files $uri $uri/ =404;
        }
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }
        location ~ /\.ht {
                deny all;
        }
}
```
**注意! 务必添加`/_h5ai/public/index.php`到默认index队列**
否则nginx找不到你的index位置.

Now you need to enable the configuration, make a symlink to the enabled sites: [original link (digitalocean support)](https://www.digitalocean.com/community/questions/how-to-create-subdomain-with-nginx-server-in-the-same-droplet)

```bash
ln -s /etc/nginx/sites-available/h5ai.hinataa.tk /etc/nginx/sites-enabled/h5ai.hinataa.tk

service nginx restart
```

## 等待全球dns服务器刷新...