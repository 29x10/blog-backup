title: python跨平台开发环境
date: 2014-08-22 00:30:34
tags:
- python
- vagrant
- virtualbox
---
<!-- more -->
##起因

###windows

1. 需要VS，依赖的编译windows只提供的VS的编译
    * MySQL-python需要C Connector，这个安装MySQL的时候是    没有给你的，你必须手动下载，另外因为windows32位和64位的区别，Program Files的路径是不一样的。所以需要手动下载MySQL-python的包，然后修改build的C Connector的路径。然后你还需要VS2008这个东西，如果版本高于2008，还需要修改环境变量模拟VS2008，最后才能成功，然后你整个人都不好了。
    * Gevent依赖libevent，然后你脸绿了。
apt-get install build-essential或者yum groupinstall "Development Tools"毫无压力
2. 安装环境繁琐，装个MySQL老大难，各种点下一步，然后还是可能还会出问题。万一开发环境复杂，需要redis、memcache、mongodb、git之类的，你脸又绿了。
3. 再说说cmd的问题，没有自动补全，完全是悲剧，右键以管理员权限运行
4. Python安装
    * setuptools需要手动下载ez_setup.py，然后手动安装
    * virtualenvwrapper-win几百年不更新了
    * 环境变量要把根目录和Scripts目录都加进去，然后你默默的看着那一坨屎一样的环境变量


###linux
1. apt-get install build-essential 或者 yum groupinstall "Development Tools"毫无压力
2. apt-get install 或者 yum install 毫无压力
3. bash毫无压力，sudo解决权限，export轻轻松松搞定环境变量


##使用Vagrant

*Pycharm本身支持，所有操作不用离开IDE，不用管VirtualBox做了什么，在做什么。*

###依赖下载

* [Vagrant官网](http://www.vagrantup.com/), 点击[下载](http://www.vagrantup.com/downloads.html)安装
* VirtualBox[下载](https://www.virtualbox.org/wiki/Downloads), 千万别傻用什么VMware


###使用说明
* `vagrant init`初始化，生成一个配置文件
* `vagrant up` 启动虚拟机
* `vagrant halt` 停止虚拟机
* `vagrant ssh` ssh到虚拟机
* `vagrant box list` 列出所有的box
* `vagrant box add <box_name>` box名字从[VagrantCloud](https://vagrantcloud.com/discover/featured)获取
* `vagrant box remove <box_name>` 删除box
* `vagrant destroy` 删除虚拟机



说到这了，开始讲怎么用把

vagrant box add ubuntu\trusty64

先随便用Pycharm建立一个项目，从Settings->Project Settings->Vagrant

![vagrant Settings](http://ww4.sinaimg.cn/large/4253447agw1ejkrepcdksj21ay0l4di9.jpg)

输入你的vagrant可执行脚本的位置

然后从Tools->Vagrant->Init in Project Root

![vagrant init](http://ww2.sinaimg.cn/large/4253447agw1ejkrl2ivybj20pa0rc77x.jpg)


这个时候根目录下面会生成一个Vagrantfile的配置文件，修改config.vm.box = "base"到config.vm.box = "ubuntu/trusty64"

然后Tools->Vagrant->Up

![vagrant up](http://ww4.sinaimg.cn/large/4253447agw1ejkrpj9q2bj20p60qa0yb.jpg)


这个时候就已经可以ssh进去了，Tools->Start SSH Session

![vagrant ssh](http://ww4.sinaimg.cn/large/4253447agw1ejkrsm55m1j20g60gamz9.jpg)

这个时候你只要把环境搭好，然后选择remote python interpreter，然后选择python的路径就可以了，这里不推荐使用系统级python，因为可以从dist-packages里看出来有很多别的依赖，环境不干净，希望大家使用virtualenv来创建一个干净的环境。

如果使用buildout的话，没什么问题，但是使用pip的话，会有缓存问题，推荐使用Pycharm进行依赖的安装。
