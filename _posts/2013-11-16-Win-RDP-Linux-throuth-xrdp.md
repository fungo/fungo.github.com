---
layout: post
title: "Windows 通过 xrdp 远程登录 Ubuntu 12.04"
date: 2013-11-16 14:22:13 +0800
categories: 技术 Linux
---

说明
====
因为老板的需要，给他开在 KVM 上开 Linux 虚拟机，又好久不用 Linux了，不想一直看着黑终端，想用桌面，于是需求就来了，如何 Window 远程登录 Linux。这里用 xrdp。

xrdp 安装
====
非常简单，直接`apt-get install xrdp`

`netstat -ntlp` 可以看到 3389 端口已经起来了。

然后就可以通过 Windows 的远程桌面来登录了

其它问题
====
1. ubuntu 桌面   
    安装的是 ubuntu server, 默认是没有桌面的，apt-get install gnome-panel 
2. xrdp 连接上后桌面进入的是 gnome-shell   
    `sudo gedit .xsession` 写入下面内容
        
        gnome-session --session=gnome-fallback
    
    然后重启 xrdp, `server xrdp restart`
3. xrdp 登录后环境变量不对
    在 .bashrc 最后加入 `. /etc/environment` 或者添加
        
        export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

参考
=====
[Linux 上的遠端桌面中繼程式: xrdp][1]   
[设置Ubuntu 12.04 Unity返回到经典Gnome桌面][2]  
[Remote Desktop - Accessing XRDP from Windows][3]  
[Ubuntu haves a different PATH when access via XRDP session][4]  
[Ubuntu Server 12.04 xrdp failed to load session 'ubuntu'][5]  

[1]:    http://www.vixual.net/blog/archives/524 "Linux 上的遠端桌面中繼程式: xrdp"
[2]:    https://tumutanzi.com/archives/7771 "设置Ubuntu 12.04 Unity返回到经典Gnome桌面"
[3]:    http://www.howtoforge.com/forums/showthread.php?t=59325 "Remote Desktop - Accessing XRDP from Windows"
[4]:    http://askubuntu.com/questions/92333/ubuntu-haves-a-different-path-when-access-via-xrdp-session "Ubuntu haves a different PATH when access via XRDP session"
[5]:    http://kosiara87.blogspot.jp/2012/06/ubuntu-server-1204-xrdp-failed-to-load.html "Ubuntu Server 12.04 xrdp failed to load session 'ubuntu'"