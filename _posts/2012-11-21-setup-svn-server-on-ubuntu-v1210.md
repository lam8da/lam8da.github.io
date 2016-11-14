---
layout: post
title: Ubuntu12.10搭建svn服务器
categories:
    - 环境&配置
tags:
    - Apache
    - SVN
    - Ubuntu
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

首先安装必要包，初始化目录：

```sh
sudo apt-get install apache2 libapache2-svn subversion
sudo mkdir /home/svn
```

创建svn项目：

```sh
sudo svnadmin create /home/svn/Medusa
```

启动svn服务：

```sh
svnserve -d -r /home/svn
```

现在可以本地访问了：

```sh
cd ~/Tmp/
svn co svn://localhost/Medusa
```

但是不能commit，如：

```sh
cd Medusa
touch localtest && svn add localtest
svn ci -m 'local test'
svn: 提交失败(细节如下):
svn: 认证失败
```

这是因为没设置用户访问权限，于是sudo编辑3个文件，例如：

```sh
cd /home/svn/Medusa/conf
```

1. 打开svnserve.conf，把：
   ```ini
   anon-access = read
   auth-access = write
   password-db = passwd
   authz-db = authz
   ```

   前面的注释删掉
2. 打开passwd，`[users]`{:.language-ini}下面添加：

   ```ini
   lambda-svn = 123456
   ```

   意思是添加一个svn的用户（不是linux系统用户）lambda-svn，密码为123456
3. 打开authz，`[groups]`{:.language-ini}下面添加：

   ```ini
   [/]
   lambda-svn = rw
   * =
   ```

   > 其中/表示项目根目录/home/svn/Medusa？
     {: .lambda_question}
   
   下面一句表示用户lambda-svn可以读写这个目录的所有文件，`* = `{:.language-ini}表示其他所有用户什么也不能干。

现在再来测试下（注意不要把svnserve kill掉）：

1. 测试本地提交：

   ```sh
   cd ~/Tmp/Medusa/
   svn ci -m 'local test'
   ```

   提示输入用户名密码时输入lambda-svn和123456，然后出错：

   ```sh
   svn: 不能打开文件“/home/svn/Medusa/db/txn-current-lock”: 权限不够
   ```

   这次的错误和第一次提交时的错误不同了，先不管，看下一个测试。
2. 测试重新checkout：

   ```sh
   cd ..
   svn co svn://localhost/Medusa Medusa1
   ```

   发现是可以的，这是因为svnserve.conf中设置了`anon-access = read`{:.language-ini}，即匿名用户可以读。注意，虽然authz里面写了`* = `{:.language-ini}，但是匿名用户仍然所可以访问的，要令匿名用户也不可已读的话需要吧svnserve.conf中设置了`anon-access = none`{:.language-ini}。（我想应该是这样，但是我测试了一下貌似设置了none之后还是可以匿名访问。。。狂汗一个`= =|||`）而authz里面的设置只是针对认证用户，即passwd里面设定的用户。

现在来解决权限不够的问题，原因在于我们是用当前系统用户（假定为current-user）创建的svnserve进程，而current-user对/home/svn/Medusa/db/txn-current-lock没有写的权限。解决这个问题有两个方法：

1. 使用超级管理员身份来启动svnserve：

   ```sh
   sudo svnserve -d -r /home/svn
   ```

   这个方法非常简单快捷
   
   > 但是不知到会不会出漏洞？
     {: .lambda_question}
2. 令current-user对/home/svn/Medusa/db/txn-current-lock有写权限。这个方法非常繁琐，自己也没真正弄清楚，还是下次重新看看Unix高级环境编程再说了。

这样之后，在本地是可以co和ci了，但是远程还是不行。需要设置/etc/apache2/mods-available/dav_svn.conf，在最后加上：

```apache
<Location /Medusa>
DAV svn
SVNPath /home/svn/Medusa
AuthType Basic
AuthName "Medusa"
AuthUserFile /home/svn/Medusa/conf/passwd

# 如果想每次登陆都输入密码请把这个标签对引掉
<LimitExcept GET PROPFIND OPTIONS REPORT>
Require valid-user
</LimitExcept>
</Location>
```

然后貌似要重启apache服务：`sudo service apache2 restart`{:.language-sh}， 就可以了。
