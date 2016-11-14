---
layout: post
title: Ubuntu12.04开机自动发邮件程序
categories:
    - 环境&配置
tags:
    - Python
    - Upstart
    - Ubuntu
---

因为实验室有台服务器装了Ubuntu，而我需要经常在宿舍远程过去，但机器可能重启（断电etc），而由于ip地址由学校的网关分配，重启后ip可能跟原来的不一样

> 也许可以设置静态ip？这样也不是每次都能成功吧？冲突了咋办？以后再研究研究
  {: .lambda_question}

，于是自己写了个python脚本，并学习了一下ubuntu的upstart机制，让该服务器每次启动ubuntu时自动给我的邮箱发一封邮件，告诉我它的ip地址。

随便找个目录，例如我的home目录/home/lambda，建立auto_mail.py文件，写下如下程序：

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
 
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email.mime.text import MIMEText
 
# python 2.3.*: email.Utils email.Encoders
from email.utils import COMMASPACE,formatdate
from email import encoders
 
import os
import socket
import fcntl
import struct
import time
 
#server['name'], server['user'], server['passwd']
def SendMail(server, from_addr, to_addr, subject, text, files=[]):
  assert type(server) == dict
  assert type(to_addr) == list
  assert type(files) == list
 
  msg = MIMEMultipart()
  msg['From'] = from_addr
  msg['Subject'] = subject
  msg['To'] = COMMASPACE.join(to_addr) #COMMASPACE==', '
  msg['Date'] = formatdate(localtime=True)
  msg.attach(MIMEText(text))
 
  for file in files:
    part = MIMEBase('application', 'octet-stream') #'octet-stream': binary data
    part.set_payload(open(file, 'rb'.read()))
    encoders.encode_base64(part)
    part.add_header(
        'Content-Disposition',
        'attachment; filename="%s"' % os.path.basename(file))
    msg.attach(part)
 
  import smtplib
  smtp = smtplib.SMTP(server['name'])
  smtp.login(server['user'], server['passwd'])
  smtp.sendmail(from_addr, to_addr, msg.as_string())
  smtp.close()
 
def GetLocalIp(ifname):
  try:
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    return socket.inet_ntoa(
        fcntl.ioctl(
          s.fileno(),
          0x8915,  # SIOCGIFADDR
          struct.pack('256s', ifname[:15])
          )[20:24])
  except Exception,ex:
    return 'exception'
 
def SentLocalIp():
  try:
    host_name = socket.gethostname()
    local_ip_0 = GetLocalIp('eth0')
    local_ip_1 = GetLocalIp('eth1')
    server = {}
    server['name'] = 'smtp.163.com'
    server['user'] = 'your-email-address-from@163.com'
    server['passwd'] = 'your-password'
    SendMail(
        server,
        'your-email-address-from@163.com',
        ['your-email-address-to@163.com'],
        host_name,
        local_ip_0 + ', ' + local_ip_1)
  except Exception,ex:
    time.sleep(1)
 
if __name__=="__main__":
  i = 0
  i_pow = 1
  while i < 20:
    SentLocalIp()
    time.sleep(60 * i_pow)
    ++i
    i_pow *= 2
```

然后在/etc/init/下面建一个auto-mail.conf，内容如下：

```sh
description "Used to send the local ip address to lambda's mailing box."
author "lambda"
start on runlevel [2345]
stop on runlevel [!2345]
script
  chdir /home/lambda/
  nohup python /home/lambda/auto_mail.py &
end script
```

这样，在开机时就会自动调用auto_mail.py了。当然也可以手动sudo service auto-mail start/stop来控制他，也可以sudo initctl status等参数来做一些别的事情。

注意事项：

1. 因为必须在登录服务器前（即ubuntu启动后但在任何用户登入前）得到ip地址（不然没有ip地址怎么ssh来登陆呢？），所以要使用upstart机制，而不能只把写好的脚本加到.profile或.bash_xxx等里面（这些文件在用户登陆后才执行）。
2. 要进行异常的捕捉，不然虽然启动就能运行程序，但是万一发生异常（例如网卡还没有启动dhcp取得ip地址）就会挂掉，从而收不到邮件了

   > 也许可以通过设置upstart的dependence来避免？这个太复杂鸟先不了他
     {: .lambda_question}

   。。。
3. 我原来写的python程序是每1分钟就发一封邮件，然后连发15次，这么做是因为可能第一次循环的时候eth0还没有ip地址而会发生异常，于是要多试几次。运行的时候发现这个程序不是只发了15次，而是发完15次后又被启动，然后再发。这样看来，我的邮箱很可能等一下就爆了。。也许要设置upstart的stop on condition来避免这个问题。今天就先不管他了，于是偷个懒设置了i_pow这个指数退避。。。
