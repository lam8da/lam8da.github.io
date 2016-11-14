---
layout: post
title: Ubuntu12.04安装CUDA5.0
categories:
    - 环境&配置
tags:
    - Cuda
    - Ubuntu
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: sh \}" /}

按照官方的安装文档来安装：下载all-in-one的run包，然后stop lightdm，然后sudo模式下安装，然后就遇到以下问题：

1. 装完cuda之后lightdm无法启动，总是出错，从syslog看到返回1。
   - 我的解决方法：先sudo X -configure然后sudo mv ~/xorg.conf.new /etc/X11/xorg.conf。虽然第一个命令的结果报错，说显示设备的数目和显卡数目不一致，但是还是会在home里面自己的目录下生成那个new文件的。
2. 安装完cuda后可以编译但是无法运行，提示找不到共享库libcudart.so.5.0：

   ```sh
   error while loading shared libraries: libcudart.so.5.0: cannot open shared object file: No such file or directory.
   ```

   - 解决方法：在/etc/ld.so.conf最后加入一行/usr/local/cuda-5.0/lib64，然后sudo ldconfig一下。
   - 其实更好的做法是：首先我们可以看到/etc/ld.so.conf中有如下语句：

     ```nothing
     include /etc/ld.so.conf.d/*.conf
     ```

     而且/etc/ld.so.conf.d/目录中有个软连接
     
     ```sh
     nvidia_settings.conf -> /etc/alternatives/nvidia_settings_conf
     ```
     
     ，而跟踪之又有：

     ```sh
     /etc/alternatives/nvidia_settings_conf -> /usr/lib/nvidia-settings/ld.so.conf
     ```

     而最后这个文件是不存在的，于是我们建立之并加上一行

     ```sh
     /usr/local/cuda-5.0/lib64
     ```

     再运行ldconfig即可。
     写那个cuda的conf文件然后ldconfig一下？
3. 运行cuda程序的时候说没有cuda capable device。
   - 解决方法：很可能是因为运行时没加sudo。我一开始便是这样的。但是等ldconfig过后就不用加sudo了。没空仔细想为什么了。。

[2012-11-21]补充：

其实第二个问题是由于安装完cuda后没运行ldconfig引起的，今天在32位机器上重新装了一遍，装完后设好PATH和LD_***之后sudo ldconfig了一下，就一点事都没有了。
