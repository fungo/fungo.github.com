---
layout: post
title: "OpenStack Horizon 开发环境搭建"
date: 2013-11-03 20:18:52 +0800
categories: 技术 OpenStack
---

背景
====
最近团队准备对 Horizon 进行定制开发，不可在现有的跑的环境上直接修改，因为其它人在用这个环境；用 DevStack 的话又太麻烦，重启的话环境还容易出问题，界面开发人员需要的真是 Horizon 这个功能，不符合小而美的原则。因此就尝试搭建独立的 Horizon 开发环境。

尝试一，官方教程(失败)
====
按照[官方的指导](http://docs.openstack.org/developer/horizon/ "Horizon Quickstart")，先 clone 项目，然后安装测试和开发环境。

在安装的过程种遇到了一些问题

1. 安装慢，一直失败
    安装非常之慢，从来没有一次成功过，都是在下载的过程中失败的。后来查看 ` ./run_tests.sh` 脚本，发现真正的安装是在 `tools / install_venv.py` 脚本里完成的，用 `pip install` 来做的，所以就手动安装这些依赖。先把 pip 包下载下来，然后 pip 命令安装。
2. 文档信息不全
    有些准备工作没有提到，比如 Horizon 用 less 来生成 css，less 需要安装 nodejs 来支持，还有默认情况下 Ubuntu 系统是没有一些头文件的，而在安装依赖的过程中，会编译一些库，需要头文件，没有话直接报错。上面提到的问题，提前安装必要的软件即可解决
        
        apt-get install nodejs
        apt-get install python-dev libmxml-dev
3. 依赖库版本有问题
    安装完成后，环境能跑起来，有登录界面，并且连上了已有的 keystone endpoint，但是无法登录，后来发现是版本问题，依赖文件 `pip-requires` 所指的包的版本和  keystone endpoint 环境下的包版本不一致。可是我测试环境安装的是 Grizzly 版，horizon clone 出来的包也是 Grizzly 生版，可是两者的依赖却不一样，郁闷，索性放弃这种方法。

尝试二， 在现有环境安装(成功，不推荐)
====
其实这个也算是我对尝试一中遇到的问题3的一个验证，我直接在跑有 horizon 的 controller 节点 clone 了一份代码，直接包，因为当初安装 horizon 的时候就已经把依赖装在系统目录里了，唯一需要做安装的就是  nodejs， `apt-get install nodejs` 搞定，然后启动开发环境，测试成功，登录正常功能也正常。

这个方法虽然可行，但有一个问题是测试环境要安装在 Controller 节点，这个是不行的，测试要与生成环境分离；并且界面开发人员有多个，我们不可能把他们所有的测试环境都建在 Controller 上面，一来 Controller 资源不够，二来开发环境没有隔离。

尝试二， apt-get安装依赖(成功，推荐)
====
有了方法二的成功经验，就想到我完全可以另外开一台虚拟机，在上面 `apt-get install openstack-dashboard` horizon 的环境给配后，然后从 github clone 一份代码出来，直接启动测试环境。事实证明是完全可以的。

详细的安装过程  

1. 虚拟机环境准备
    这个过程和一般的 OpenStack 安装节点环境准备一样，包括源的添加，系统的更新

2. 安装包

        apt-get install nodejs
        apt-get install openstack-dashboard
3. clone 代码

        git clone https://github.com/openstack/horizon
        cd horizon
        git checkout stable/grizzly

4. 关闭 apache 服务
    不需要这个，就必要让它一直跑着浪费资源
    
        service apache2 stop
        update-rc.d apache2 disable


总结
====
最终开发环境配置成功，推荐使用方法3来做。