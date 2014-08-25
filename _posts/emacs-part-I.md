title: Emacs For Python插件安装(上篇)
date: 2012-04-02 13:22
tags:
- emacs
- python
---
<!-- more -->
感谢<http://www.cnblogs.com/coderzh/archive/2009/12/26/emacspythonide.html>
提供了技术支持

###下面介绍的均在MacOs10.7下面进行
首先,为了保证Emacs的可执行路径以及PATH的环境变量
``` cl the executable path(exec-path)
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(current-language-environment "UTF-8")
 '(desktop-base-file-name "emacs.desktop")
 '(desktop-save t)
 '(desktop-save-mode t)
 '(exec-path (quote ("/usr/bin" "/bin" "/usr/sbin" "/sbin" "/Applications/Emacs.app/Contents/MacOS/bin" "/usr/local/bin/" "/opt/local/bin" "/opt/local/sbin" "/Users/jerome/Documents/apache-maven-2.2.1/bin" "/sbin:/usr/local/bin" "/usr/X11/bin" "/Library/Python/2.7/site-packages/django/bin" "/usr/local/mysql/bin"))) ;; this line
 '(show-paren-mode t))
 (custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
```
``` sh set the PATH environment for emacs
(setenv "PATH" "/opt/local/bin:/opt/local/sbin:/Users/jerome/Documents/apache-maven-2.2.1/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/X11/bin:/Library/Python/2.7/site-packages/django/bin:/usr/local/mysql/bin")
```
###模板插件YASnippet
官方网站，之前是放在google code上面的，现在迁移到GitHub上面了

[github repo](https://github.com/capitaomorte/yasnippet)

安装方式见官方网站的帮助
我也比较喜欢把插件都放在~/.emacs.d/plugin/plugin-name/
下面开始安装
``` sh git clone repo
cd ~/.emacs.d/plugin
git clone https://github.com/capitaomorte/yasnippet
```
``` cl 然后添加到你的.emacs
(add-to-list 'load-path
    "~/.emacs.d/plugin/yasnippet")
(require 'yasnippet) ;; not yasnippet-bundle
(yas/global-mode 1)
```

这个功能跟TextMate很类似，敲出关键词，然后按TAB就生成模板了
至于他的Expand模式，我会继续研究，稍后给大家释出，方便大家以后的工作,着急的可以先睹为快

1. <http://capitaomorte.github.com/yasnippet/snippet-expansion.html>
2. <http://capitaomorte.github.com/yasnippet/snippet-development.html>
3. <http://capitaomorte.github.com/yasnippet/snippet-menu.html>
4. <http://capitaomorte.github.com/yasnippet/snippet-organization.html>

###知名插件auto-complete

[官方网站](http://cx4a.org/software/auto-complete/manual.html)
[下载链接](http://cx4a.org/pub/auto-complete/auto-complete-1.3.1.zip)

安装方法

1. 打开emacs
2. M-x load-file RET
3. 键入你auto-complete解压缩之后的地址/etc/install.el RET
4. 之后键入你要安装的地址,比如我是~/.emacs.d/auto-complete
5. 等待安装完成
``` cl 更新.emacs
;; for auto-complete
(add-to-list 'load-path "~/.emacs.d/plugin/auto-complete/")
(require 'auto-complete-config)
(add-to-list 'ac-dictionary-directories "~/.emacs.d/plugin/auto-complete//ac-dict")
(ac-config-default)
(define-key ac-mode-map (kbd "M-TAB") 'auto-complete) ;;我绑定的快捷健
```
###重构工具Rope安装
``` sh
sudo easy_install rope
```
Pymacs安装

[官方网站](http://pymacs.progiciels-bpi.ca/pymacs.html)

[gitrepo](https://github.com/pinard/Pymacs)

解压缩之后编辑下Makefile
修改EMACS和PYTHON修改到你对应的版本
下面是我的
```
EMACS = /Applications/Emacs.app/Contents/MacOS/Emacs
PYTHON = python
```
之后到当前目录下执行
``` sh
sudo make install
```
然后把目录下的pymacs.el复制到~/.emacs.d/plugin/pymacs下面
```
M-x load-library RET pymacs RET
```
这个时候不应该报错
``` sh 更新.emacs
(autoload 'pymacs-apply "pymacs")
(autoload 'pymacs-call "pymacs")
(autoload 'pymacs-eval "pymacs" nil t)
(autoload 'pymacs-exec "pymacs" nil t)
(autoload 'pymacs-load "pymacs" nil t)
```
- 重启emacs
- M-x pymacs-eval RET repr(2L**111) RET
- 结果应该是2596148429267413814265248164610048L
- M-x pymacs-load RET os RET RET
- M-: (os-getcwd) RET
- 最后应该得到当前buffer目录

然后我们继续

- 安装ropemacs
- 前提是You should install rope library, ropemode and pymacs before using ropemacs

``` sh
sudo easy_install ropemode
```

安装

[官方网站](https://bitbucket.org/agr/ropemacs)

下载后解压进入目录
``` sh
sudo python setup.py install
```
安装之后更新.emacs
``` cl
(require ‘pymacs)
(pymacs-load “ropemacs” “rope-”)
```
重启后确定没有问题
