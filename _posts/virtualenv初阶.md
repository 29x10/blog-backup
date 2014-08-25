title: virtualenv初阶
date: 2012-04-03 16:27
tags: python
---
###virtualenv
类似java下面的maven

1. 包的版本号
2. 不同的config的repo的位置也不一样

当然了，maven的好处就是所有包丢到一个仓库里没有问题，只要maven配置的时候写上指定的版本号就可以了
<br />python下面要怎么去做呢？这就要提到virtualenv了，当然了，我们也需要pip的帮助
<br />先来看看怎么做吧
最开始当然就是安装了,没有pip的可以先把pip装上(easy_install对我们来讲不够用了)
``` sh
sudo easy_install pip
sudo pip install virtualenv
```
完成之后我们就可以创建我们想要的环境了
``` sh
virtualenv --no-site-packages --python=python2.7 test_env && cd test_env
```
然后我们就能看到bin, lib, include

- bin 一些可执行的xxx了，包括python
- include python对应版本的头文件
- lib python对应版本的模块,lib/site-package下面就是我们eggs的存放目录了

下面讲下基本的参数吧

- `--no-site-package`不去试用global下面的site-package,默认参数，无需去管
- `--python` specify which version you would like to use
- `--distribute` Use Distribute instead of Setuptools 我基本不管这个参数，主要是不会用- -
- `--system-site-packages` use global site-package
- `--relocatable` Make an EXISTING virtualenv environment relocatable. This fixes up scripts and makes all .pth files relative,每次装完新包都要`virtualenv --relocatable env_name`, 还有一点要注意 *If you use this flag to create an environment, currently, the --system-site-packages option will be implied.*
- `--extra-search-dir` 变更搜索的dir，这个一般是在你离线的情况下去用的`Setuptools` distributions must be `.egg` files; `distribute` and `pip` distributions should be `.tar.gz` source distributions.
- `--never-download` 不通过网络下载

大概就这些了，其他的基本很少用到
<br/ >[Bootstrap Scripts](http://www.virtualenv.org/en/latest/index.html#creating-your-own-bootstrap-scripts)这个东西我自己也不太懂，好像很复杂的样子，而且我自己也用不到，以后研究好我会post出来的，有兴趣的同学可以尝试，多多交流哈

###终于开始讲使用了呢
创建完的第一件事情就是激活我们的虚拟独立环境了
<br />确认下是否已经进入环境目录了
``` sh
source bin/activate
```
这样我们就进入了环境，如何退出呢
```
deactivate
```
然后是包的安装了

- `pip install package_name` you can also sepcify the package version, eg.`pip install package_name==x.x`
- `pip uninstall package_name`
- `pip search package_name`

安装的东西这些应该就够玩了
<br />版本指定我们已经看到了 ，独立环境我们也已经解决了，那么如何协同开发呢
<br />我们可以看到maven是有包的树状结构的,这个我们也可以做到
``` sh
lssitepackage
```
我们能看到装了什么包，更详尽的数据可以通过pip来解决`pip freeze`, 我们能看到包名跟版本号,牛逼的要来了
```
pip freeze > requirements.txt      ##名字自己取
```
拿到这个输出有什么用呢？
<br />svn或者git上去的东西不可能把整个环境commit上去吧= =
<br />但是requirements.txt是可以丢上去的，又不大
<br />拿到别人的机器上
```
pip install -r requirements.txt
```
别人起一个virtualevn, 拉下代码，跑下上述代码，Ok了，依赖都没有问题了
```
看下不足吧，没有maven的exclude，不过够用了
