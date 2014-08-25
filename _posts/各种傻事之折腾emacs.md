title: 各种傻事之折腾emacs
date: 2012-04-06 00:38
tags:
- markdown
- emacs
---
###markdown插件
先讲一点啊，markdown的时候强烈建议不要用`<br />`，宁愿多敲回车
``` sh
cd ~/.emacs.d/plugin
git clone git://jblevins.org/git/markdown-mode.git
```
修改.emacs
``` cl
(add-to-list 'load-path "~/.emacs.d/plugin/markdown-mode")
(autoload 'markdown-mode "markdown-mode.el"
  "Major mode for editing Markdown files" t)
(setq auto-mode-alist (cons '("\\.markdown" . markdown-mode) auto-mode-alist))
```
我基本只是用来看语法高亮的，其他的无视了
###重搞emacs
之前说过通过`ln -s`来解决全局emacs的情况, 但是要注意一点，因为emacs的路径是hardcode的,连接emacs的话，他会去找你编译的时候的路径，而且lib也奇奇怪怪的
```
Warning: arch-dependent data dir (/Users/jerome/Desktop/trunk/nextstep/Emacs.app/Contents/MacOS//libexec/emacs/24.0.92/x86_64-apple-darwin11.2.0/) does not exist.
Warning: arch-independent data dir (/Users/jerome/Desktop/trunk/nextstep/Emacs.app/Contents/Resources/share/emacs/24.0.92/etc/) does not exist.
Error: charsets directory (/Users/jerome/Desktop/trunk/nextstep/Emacs.app/Contents/Resources/share/emacs/24.0.92/etc/charsets) does not exist.
Emacs will not function correctly without the character map files.
Please check your installation!
Warning: Could not find simple.el nor simple.elc
Fatal error (6)Abort trap: 6
```
解决方法来了
``` sh
sudo rm /opt/local/bin/emacs
sudo rm /opt/local/bin/emacsclient
```
重命名下`/Applications/Emacs.app/Contents/MacOS/Emacs`到emacs

修改.bash_profile
``` sh
export PATH=/Applications/Emacs.app/Contents/MacOS/:/Applications/Emacs.app/Contents/MacOS/bin/:$PATH
```
即可解决，而且大家可能也注意到了一点，因为之前virtualenvwrapper的emacs插件，平常我们只需要启动emacsclient编辑即可，基本是把emacs丢到另外一个桌面去搞的

编辑完毕可以`C-x k RET yes RET`退出

之前hardcode的问题时候做了软连接，然后删除软链接的时候`rm -rf`了，然后emacs就88了，重新编译
``` sh
bzr branch bzr://bzr.savannah.gnu.org/emacs/trunk
cd trunk
./autogen.sh
./configure --with-ns
make
sudo make install
```
从nextstep目录下复制Emacs.app就可以了

然后启动无压力
