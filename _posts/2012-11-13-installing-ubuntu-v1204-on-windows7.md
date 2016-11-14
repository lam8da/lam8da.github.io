---
layout: post
title: Windows7下使用EasyBCD硬盘安装Ubuntu(12.04)
categories:
    - 环境&配置
tags:
    - Windows7
    - Ubuntu
    - 系统安装
---

1. 首先划分出一个空的分区出来（一定要是主分区或者逻辑分区，Win7自创的所谓简单分区是不行的！），最好达到20G以上吧。
2. 准备两个东西EasyBCD软件和iso镜像（我用的easybcd是2.2版，直接从官网下载免费版即可，据说不能是绿色版的；另外我的iso镜像是ubuntu-12.04-desktop-amd64.iso，注意即便cpu是Intel的也能使用这个amd64版进行安装，因为Intel采用的架构是amd先采用的，故iso起名为amd）
3. 安装完后打开EasyBCD软件，选择Add New Entry，选NeoGrub 标签然后点Install，接着是Configure，然后就会出现一个menu.lst文件，把下面的内容复制进去，把原来的全覆盖掉

   ```nothing
   title Install Ubuntu
   root (hd0,0)
   kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/ubuntu-11.10-i386.iso ro quiet splash locale=zh_CN.UTF-8
   initrd (hd0,0)/initrd.lz
   ```
 
   特别注意:
   - ubuntu-12.04-desktop-amd64.iso是你的iso的名字，别写成我的了，这个要改成你的。
   - 对于有的电脑上你的第一个盘符并不是C盘，在磁盘管理中可以看出，所以安装时需将(hd0,0)改为（hd0,1）（假设为第二个）。
4. 保存menu.lst然后关闭之。接下来：把准备好的iso用压缩软件或者虚拟光驱打开，找到casper文件夹，复制initrd.lz和vmlinuz到C盘,然后在把iso也拷贝到C盘。
5. 重启后，就会看到有2个启动菜单，我们选择第2个（NeoGrub这个），然后选Install Ubuntu（即刚才我们在menu.lst上输入的title），然后等待一段时间，就会见到我们日思夜想的ubuntu了。
6. 按Ctrl+Alt+T打开终端，输入代码：sudo umount -l /isodevice。这一命令取消掉对光盘所在驱动器的挂载，否则分区界面找不到分区。
7. 默认桌面有2个文档，一个是演示的不用管，我们选择安装Ubuntu，开始安装，选语言不用说，选安装类型，我们用其他选项（自定义）。然后一路装下去即可。
8. 最后进入Window7，打开EasyBCD删除安装时改的menu.lst文件，按Remove即可。然后去我们的c盘 删除vmlinuz，initrd.lz和系统的iso文件。

注意：

- 在选择安装启动引导器的设备时，一定要选在原来windows系统引导程序所在磁盘的第一个分区！（直接选那块磁盘就行了）
- 若重启后发现原来windows进不去了，则需要打开终端输入命令：sudo gedit /etc/default/grub，然后修改GRUB_TIMEOUT=\"10\"，然后在终端中输入sudo update-grub。update 命令会自动找到 windows 7 启动项。并且自动更新 /boot/grub/grub.cfg 文件。这样重启就能进windows了。
- 利用EasyBCD可以更改启动项菜单按Edit Boot Menu按钮，可以选择将Windows7设为默认开机选项。
