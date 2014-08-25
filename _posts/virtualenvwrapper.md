title: virtualenv进阶之virtualenvwrapper
date: 2012-04-03 20:52
tags: python
---
先贴官网吧，一般不好找[传送门](http://www.doughellmann.com/docs/virtualenvwrapper/)
``` sh
sudo pip install virtualenvwrapper
export WORKON_HOME=~/Envs
mkdir -p $WORKON_HOME
source /usr/local/bin/virtualenvwrapper.sh
```
修改bash_profile
``` sh
export WORKON_HOME=~/Envs
source /usr/local/bin/virtualenvwrapper.sh
```
- 创建环境就是`mkvirtualenv env_name`
- 参数之类的参考`virtualenv`
- 所有的环境均在`$WORKON_HOME`下

任何地方调用`workon`可显示所有的环境
</ br>`workon env_name`即可进入当前的环境

### 基本调用
- `lssitepackages`   list the site-package you installed
- `rmvirtualenv env_name` remove the env_name
- `cpvirtualenv env_1 env_2` copy the env_1 to env_2
- `mkproject env_name` Create a new virtualenv in the `WORKON_HOME` and project directory in `PROJECT_HOME`

### For Django users (deprecated)
``` sh
sudo pip install virtualenvwrapper.django
mkproject -t django django_project_name
```
## Note: here would be a conflict with the following essential plugin
```
Last login: Tue Apr  3 22:59:52 on ttys005
emacs_desktop_controller_on
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  ImportError: No module named hook_loader
  virtualenvwrapper.sh: There was a problem running the initialization hooks. If Python could not import the module virtualenvwrapper.hook_loader, check that virtualenv has been installed for VIRTUALENVWRAPPER_PYTHON=/usr/bin/python and that PATH is set properly.
```

**这里其实不是关键了，大招来了**
### Existing Extensions
[传送门](http://www.doughellmann.com/docs/virtualenvwrapper/extensions.html)
</ br>vim用户自行研究，emacs党可以继续跟下去了

[for emacs users](http://www.doughellmann.com/docs/virtualenvwrapper/extensions.html#emacs-desktop)


`desktop-mode`可以不用管，继续往下走[emacs-desktop](http://www.doughellmann.com/projects/virtualenvwrapper-emacs-desktop/)

save the state of emacs (open buffers, kill rings, buffer positions, etc.) between sessions. `说穿了就是个保存当前工作状态的东西`
``` sh
sudo pip install virtualenvwrapper-emacs-desktop
```

1. 打开emacs
2. `M-x customize-group RET desktop RET`
3. 找到`desktop-save-mode`切到`on`
4. 找到`desktop-base-file-name` 输入 `emacs.desktop` 只要确保文件名不是以`.`开头的隐藏文件就行了
5. 找到`desktop-path`, 确保你的`配置文件`的目录在这个里面,默认一般正常
6. 找到`desktop-save`, 调到`Always save`
7. 有些buffer我们并不需要保存，这里可以设置

打开你的Terminal.app, `cmd + 逗号`, **settings->shell->startup**

![terminal](http://i1192.photobucket.com/albums/aa325/kongpo0412/ScreenShot2012-04-03at104845PM.png)

- Run command: `emacs_desktop_controller_on`
- Run inside shell: `enable`

这里要讲一下的是这个利用了emacs的`server-mode`, 所以我们要确定`server-mode`是开启的
<br />修改.emacs
``` cl
(server-start)
```
然后就是因为Mac的原因了，我们的emacs基本是放在`/Applications/Emacs.app/Contents/MacOS/Emacs`的, 平时的emacs是自带的那个，所以需要重定向下

增加链接，`echo $PATH`, 找到比`which python`考前的路径,比如我是`/opt/local/bin/`
``` sh
sudo ln -s /Applications/Emacs.app/Contents/MacOS/bin/emacsclient /opt/local/bin/emacsclient
sudo ln -s /Applications/Emacs.app/Contents/MacOS/Emacs /opt/local/bin/emacs
```
然后你每次运行`workon env_name`的时候，emacs会自动切到对应的session的，`碉堡了，有木有`

### 最后是一些emacs的小菜
消除自动保存
``` cl
(setq make-backup-files nil)
(setq make-temp-file nil)
(setq delete-old-verson 1)
```
**远程编辑文件**
`C-x C-f`路径里输入`/protocol:username@host:path-to-file`, `protocol`支持`ssh`和`ftp`

最后呢，就是平时emacs启动太慢，修改个小文件都要等半天，怎么能忍啊，这必须不能啊

`emacs -q path-to-file` 这样就不会去加载配置文件了
