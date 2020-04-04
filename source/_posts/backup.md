---
title: backup
date: 2019-06-11 23:27:58
tags:
categories:
---

Some config file from digitalocean sever.
<!-- more -->

```
/var/www/
        ├── ftp
        │   ├── _h5ai
        │   └── files...
        ├── html
        │   ├── about
        │   ├── posts
        │   └── ...
        └── webdav
            ├── phpsite
            └── testfolder
```

```bash 
root@ubuntu-s-1vcpu-1gb-sfo2-01:/etc/apache2# l sites-enabled/
000-default.conf -> ../sites-available/000-default.conf
dav.hinataa.tk.conf -> ../sites-available/dav.hinataa.tk.conf
h5ai.hinataa.tk -> /etc/apache2/sites-available/h5ai.hinataa.tk.conf
h5ai.hinataa.tk.conf -> ../sites-available/h5ai.hinataa.tk.conf
php.hinataa.tk.conf -> ../sites-available/php.hinataa.tk.conf
```

Some files from `/etc/apache2/sites-available`

 - 000-default.conf
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com
        ServerName hinataa.tk
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        ErrorDocument 404 /static/404.html
        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

 - h5ai.hinataa.tk.conf
```
<VirtualHost *:80>
        ServerName h5ai.hinataa.tk
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/ftp

        #ErrorLog ${APACHE_LOG_DIR}/error.log
        #CustomLog ${APACHE_LOG_DIR}/access.log combined
        DirectoryIndex  index.html  index.php  /_h5ai/public/index.php
        <Directory "/var/www/ftp">
            AllowOverride all
            Order allow,deny
            Allow from all
        </Directory>
</VirtualHost>
```

 - dav.hinataa.tk.conf
```
DavLockDB /var/www/DavLock
<VirtualHost *:80>
        ServerName dav.hinataa.tk
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/webdav/
        <Directory "/var/www/webdav/">
            DAV ON
            AuthType Digest 
    AuthName "webdav" 
            AuthUserFile /etc/apache2/webdav.passwords 
            Require valid-user
        </Directory>
</VirtualHost>
```