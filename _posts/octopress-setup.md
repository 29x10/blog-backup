title: octopress搭建详解(MacOSX&amp;&amp;Fedora)
date: 2012-04-04 17:43
tags: octopress
---
<!-- more -->
### 最开始是在ubuntu下尝试的
最开始的尝试是在mac本子上的，但是因为gcc是llvm的，虽然我不清楚那代表什么，我只是知道rvm编译不了 llvm确实恶心，像之前的cxOracle的编译就是，折腾死我了 最后想想还是在parallels desktop上面部署一个ubuntu来折腾好了
``` sh
sudo apt-get install openssh-server
service ssh start
```
在ssh之前

- mac本子先vpn连出去吧，要不接下来的安装会各种莫名奇怪的down掉
- 之后mac本子直接ssh上去，然后就无视掉虚拟机吧
- 首先先处理下bash_profile的问题吧 home下是有.bashrc的

#### 习惯问题，先安装emacs了
`sudo apt-get install emacs`

之后`emacs .bashrc`

添加以下内容
``` sh
if [ -f ~/.bash_profile ]; then
    . ~/.bash_profile
fi
```
之后`touch ~/.bash_profile`

自带是没有curl的`sudo apt-get install curl`

之后就按照安装说明来吧 通过rvm来安装ruby,应该是ruby的包管理器吧,类似pip或者easy_install（setuptools） 先安装rvm并做配置

``` sh
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile
source ~/.bash_profile
```
现在rvm应该正常了,运行`rvm requirements`看下需求吧，根据平台的不同需求是不一样的，大致有几条吧

1. `apt-get`一堆包
2. 说是装1.9.2之前要1.8.7

具体我放到fedora下面去讲好了，因为后续的ubuntu卡在了heroku上面

然后这里需要注意一点了，因为gem是需要zlib这个包的， 要是不装的话，你gem install xxx的时候就会提示以下错误
```
ERROR:  Loading command: install (LoadError)
    no such file to load -- zlib
ERROR:  While executing gem ... (NameError)
    uninitialized constant Gem::Commands::InstallCommand
```
然后安装ruby的时候并不会自带安装这个包，而且如果你后续去补的话，也是无效的 所以了，先把zlib包补上吧

`rvm pkg install zlib`

因为后续的heroku的问题，还是需要把readline以及openssl的包的`先不要运行`
```
rvm pkg install readline
rvm pkg install openssl
```
这个时候readline是可能会出问题的，写的是autoreconf的问题

这里应该到此为止了，因为后续的`heroku`的`readline`一直不行，因为我没有看`rvm requirements`的东西,而是去网上查问题，装了很多5的包，所以就先这样吧，详情可以参考后面我讲`Fedora`的安装

### Fedora16
**细节的东西我记得不是很清楚了,大家也大致看下，有什么细节的问题可以问出来**
安装完毕第一件事，肯定是干掉selinux,修改`/etc/selinux/config`设置为`disable`, 或者直接运行`setenforce 0`

之后就是rpm fusion和visudo了

之后干掉gnome吧，临时的话可以`sudo init 3`，永久的话
``` sh
sudo rm -f /etc/systemd/system/default.target
ln -s /lib/systemd/system/multi-user.target /etc/systemd/system/default.target
```

多余的不讲了，直接搞,安装ssh开启并设置开机启动

```sh
sudo yum install openssh-server
sudo service sshd start
sudo systemctl enable sshd.service
```
安装rvm
``` sh
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile
source ~/.bash_profile
```
#### rvm requirements
```
bash >= 4.1 required
curl is required
git is required (>= 1.7 for ruby-head)
patch is required (for 1.8 rubies and some ruby-head's).
```
so
``` sh
sudo yum install git
sudo yum install patch
```
```
To install rbx and/or Ruby 1.9 head (MRI) (eg. 1.9.2-head),
then you must install and use rvm 1.8.7 first.
```
这里应该是有问题的`1.8.7`应该指的是`ruby`,而不是`rvm`
```
Additional Dependencies:
# For Ruby / Ruby HEAD (MRI, Rubinius, & REE), install the following:
  ruby: yum install -y gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison iconv-devel ## NOTE: For centos >= 5.4 iconv-devel is provided by glibc
```
义无反顾吧
``` sh
yum install -y gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison iconv-deve
```

考虑到之后`rake generate`的时候的动态链接库的问题
``` sh
sudo yum install python-pygments
sudo yum install python-devel
```

先安装包, autoreconf的问题就出来了，[传送门](https://rvm.beginrescueend.com/packages/readline/)
``` sh
rvm pkg install zlib
rvm --skip-autoreconf pkg install readline
rvm pkg install openssl
```
之后就先尝尝1.8.7吧
``` sh
rvm install 1.8.7
rvm use 1.8.7
```
玩1.9.2去
``` sh
rvm install 1.9.2
rvm use 1.9.2
```
出问题的话
``` sh
rvm uninstall 1.9.2
rvm pkg uninstall zlib
rvm pkg uninstall readline
rvm pkg uninstall openssl
rvm pkg install zlib
rvm --skip-autoreconf pkg install readline
rvm pkg install openssl
rvm install 1.9.2
rvm use 1.9.2
```
确定你的gem是最新的`rvm rubygems latest`

cd到你想要安装的目录
``` sh
git clone git://github.com/imathis/octopress.git octopress
cd octopress
```
**这里的话，会提示个东西，输入`y`之后回车即可**

安装bundler,并通过bundler安装gem
``` sh
gem install bundler
bundle install
```
安装默认主题`rake install`

接下来就是部署的问题了，我喜欢heroku,因为我学django就是丢到这个上面去的，[部署方式传送门](http://octopress.org/docs/deploying/)

[heroku传送门](http://www.heroku.com/)，自己注册，不解释了

`gem install heroku`,然后`heroku login`，自己的账户密码，然后如果没有上传你的sshkey的话，跟我走
``` sh
ssh-keygen -t rsa -C "your_email@youremail.com"
heroku keys:add ~/.ssh/id_rsa.pub
```
然后确定你在octopress目录下
``` sh
heroku create
# Set heroku to be the default remote for push/fetch
git config branch.master.remote heroku
```
编辑`.gitignore`去掉`public`
``` sh
rake generate
git add .
git commit -m 'site updated'
git push heroku master
```
`rake generate`是让你的改动推到public目录下面的

接下来修改域名`heroku rename name_you_like`

进入`_config.yml`，修改相关配置, [配置传送门](http://octopress.org/docs/configuring/)

这里需要注意的就是`Disqus Comments`,这是个外挂的评论，想要评论功能的可以启用

写文章的话，到`octopress`目录`rake new_post["post_name"]`, 这里可以设置是否可以`评论`, 提交的话

`rake generate && git add . && git commit -m "asdfasdfasdasd" && git push heroku master`

提交之前可以预览`rake preview`, 打开`127.0.0.1:4000`

大致就这些, preview的难度也可以解决,终端机`ssh -D 7070 username@host`,然后通过`firefox`或者`chrome`做个代理，添加下规则, 然后直接访问`host:4000`即可

当然了，这样做的话，你写文章的时候就不能通过ssh来圈圈叉叉了,vpn还是没有什么问题的

还有就是外面的东西往emacs里面贴的时候，会有奇怪的空格，复制那些空格，然后`M-shift-%`, replace之后使用`!`来全部应用

###MacOSX 10.7.3 && Xcode4.3
这样太不爽了，mac必须雄起,网上的解决方法就是`osx-gcc-installer`, 我觉得就是不爽，非要Xcode
直接贴步骤吧, 需要homebrew
```
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile
source ~/.bash_profile
rvm get head
brew install libksba
```
需要说的是`autoconf`和`automake`的问题，我以前编译emacs的时候装过

```
mac用户需要下载macport安装，brew无效亲测
port install automake
port install autoconf
```

网上还有个方法是`Xcode->Preferences->Downloads->command line tools`来安装的，我反正都一起装上了

#### 然后牛逼的一招来了

``` sh
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/
```

作用嘛，据说是`set developer directory path`, 我是因为后续的`rb-fsevent`包安装失败而找的解决方案，[thanks@clippit](https://github.com/clippit)

- [来源1](https://github.com/thibaudgg/rb-fsevent/issues/26)
- [来源2](https://github.com/thibaudgg/rb-fsevent/issues/31)

以下是官方用法

![xcode-select](http://i1192.photobucket.com/albums/aa325/kongpo0412/ScreenShot2012-04-04at82227PM.png)

之后
``` sh
rvm pkg install zlib
rvm pkg install readline
rvm pkg install openssl
rvm install 1.9.3
rvm gemset use global
rvm uninstall rake
```
关闭当前的terminal session吧，重新打开一个
``` sh
git clone git://github.com/imathis/octopress.git octopress
cd octopress
```
`emacs .rvmrc`改成1.9.3 `emacs Gemfile`, `rake`一行改成`gem 'rake', '0.9.2'`

之后`rvm use 1.9.3`
``` sh
gem install bundler
bundle install
rake install
```

heroku的问题可以去[传送门](https://toolbelt.herokuapp.com/)

然后就可以在本地圈圈叉叉了

迁移的话, `clone`之后`git remote add heroku git@heroku.com:your_app_name.git`
