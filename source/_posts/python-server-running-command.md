---
title: python监听端口 执行服务器命令
date: 2019-04-16 19:22:23
tags: python
categories: 服务器
---

## python socket & subprocess
使用`socket`模组监听端口, 并使用 `subprocess` 模组实现执行命令.

```python server.py
import socket
import subprocess

s = socket.socket()
print("[*] Server is running...")
host = 'server_ip' # 例如 host = '11.22.33.44'
port = 2333        # 自定义 和客户端一致 确保没被防火墙阻挡
s.bind((host,port))

while True:
    s.listen(0)
    client, address = s.accept()
    print('[+] connected from client:', client, address)

    p = client.recv(1024)
    if p == b'your_pass_key':
        client.send(b'[+] Access Granted.')
        subprocess.run(['rm','-rf','/var/www/html/*'])
        subprocess.run(['rm','-rf','/var/www/html/.git/'])
        subprocess.run(['git','clone','https://github.com/username/username.github.io.git', '/var/www/html/'])
        # 注意写成你的github用户名
        client.close()
        continue
    client.send(b'[-] Access Denied.')
    client.close()
```
<!-- more -->
以上是server.py的内容.

```python client.py
import socket

s = socket.socket()

host = 'server_ip' # 你的服务器ip地址
port = 2333        # 保持与监听的一致

s.connect((host, port))
print('server', s.getpeername())
s.send(b'your_pass_key')
print(s.recv(1024))
```
这是client.py的内容.

## 后台运行python脚本

```bash
chmod +x server.py
nohup python3 ./server.py &
```
**注意这个`&`符号, 不加这个是无法后台运行的**, 并且注意你的路径, 把`./server.py`换成绝对路径会更好.

如何终止后台的进程呢?
```bash
root@ubuntu-s-1vcpu-1gb-sfo2-01:~$ ps ax | grep server.py
26053 pts/0    S      0:00 python3 ./server.py
26058 pts/0    S+     0:00 grep --color=auto server.py
```

使用如上命令, 可以显示后台运行的`server.py`的进程 `PID`.
要终止该进程只需 `kill PID` 即可. (上面的grep是你输入的查找命令运行时产生的进程, 不需要kill它的PID, 你终止它也会显示没有这个PID因为他已经运行完了)
```bash
root@ubuntu:~$ kill 26053
[1]+  Terminated              python3 ./server.py      
```

## 为什么写这个呢

主要是我是使用`nginx`来主持html界面的, `/var/www/html/`里是我`hexo project`生成的经渲染过后的文件. 

现在我把主生成程序(含源.md文件)上传到一个分支, 然后travis-ci会自动帮我构建然后把渲染的`./public` 下的文件推送到master分支. 以此来生成更新我的`github pages`.

然后服务器端并不会自动更新文件, 这就需要手动ssh进服务器然后手输命令, 有一点太麻烦了. 于是就写了这个, 等travis-ci给我构建成功了我再运行一下这个`python client.py`就可以省时省力运行服务器脚本了.