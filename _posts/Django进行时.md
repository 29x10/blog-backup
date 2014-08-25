title: Django进行时
date: 2012-04-02 10:32
tags:
- django
- python
---
可根据需求调整32位还是64位python

{% codeblock switch to python 32-bit mode lang:applescript %}
defaults write com.apple.versioner.python Prefer-32-Bit -bool yes
{% endcodeblock %}

``` applescript switch to python 32-bit mode
defaults write com.apple.versioner.python Prefer-32-Bit -bool yes
```

如何确定32还是64位
我记得webpy是要求32位的，目前本人是64位

``` py test python arch, following is 64bit
import sys
sys.maxint
9223372036854775807
```

``` sh install django to site-package
sudo easy_install django
```

``` sh edit the bash_profile
export PATH=$PATH:/Library/Python/2.7/site-packages/django/bin
```

然后开始安装mysql
[download](http://dev.mysql.com/downloads/mysql/)

目前还没有10.7的安装

根据自己的选择，32还是64位，直接选择DMG安装

有两个PKG文件，先选择那个安装的，还有一个是启动的，最后双击那个Pane就好,之后选择是当前用户还是全体用户，我就简单选择当前用户了

``` sh edit the bash_profile
export PATH=$PATH:/usr/local/mysql/bin
```

``` sh 之后mysql的启动，可以通过pane启动还可以手动
sudo /Library/StartupItems/MySQLCOM/MySQLCOM start
```

``` sh install the python-mysql connector
sudo easy_install MySQL-python
```

``` sh edit bash_profile
export DYLD_LIBRARY_PATH=/usr/local/mysql/lib/
``` 



