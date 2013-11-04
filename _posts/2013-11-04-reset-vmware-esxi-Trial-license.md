---
layout: post
title: "重置VMware ESXi 4.1 试用版 License"
date: 2013-11-04 10:45:44 +0800
categories: 技术 VMware
---

前提
====
1. ESXi 启用 ssh 远程登录服务
2. 有 root 帐户

步骤
====
    ssh root@ESXi_IP
    cd /etc/vmware/
    rm vmware.lic
    rm license.cfg
    services.sh restart

等服务重启完就可以了，试用期变为60天。