title: Emacs For Python插件安装(下篇)
date: 2012-04-02 20:34
tags:
- emacs
- python
---
###pycomplete插件

[官方网站](http://www.rwdev.eu/articles/emacspyeng)

[下载python-mode](http://www.rwdev.eu/python/pycomplete/python-mode.el)

[下载pycomplete.el](http://www.rwdev.eu/python/pycomplete/pycomplete.el)

[下载pycomplete.py](http://www.rwdev.eu/python/pycomplete/pycomplete.py)

- 将python-mode.el和pycomplete.el放到~/.emacs.d/plugin/pycomplete下面
- 将pycomplete.py放到你的python site-package下面 -> /Library/Python/2.7/site-packages

**接下来需要重新更正下.emacs中涉及pymacs的配置(删掉之前的pymacs配置吧)**
``` sh
;; python-mode settings
(add-to-list 'load-path "~/.emacs.d/plugin/pycomplete/")
(setq auto-mode-alist (cons '("\\.py$" . python-mode) auto-mode-alist))
(setq interpreter-mode-alist(cons '("python" . python-mode)
                             interpreter-mode-alist))
;; path to the python interpreter, e.g.: ~rw/python27/bin/python2.7
(setq py-python-command "python")
(autoload 'python-mode "python-mode" "Python editing mode." t)

;; for pymacs
(add-to-list 'load-path "~/.emacs.d/plugin/pymacs/")
(setq pymacs-python-command py-python-command)
(autoload 'pymacs-apply "pymacs")
(autoload 'pymacs-call "pymacs")
(autoload 'pymacs-eval "pymacs" nil t)
(autoload 'pymacs-exec "pymacs" nil t)
(autoload 'pymacs-load "pymacs" nil t)
(require 'pycomplete)

;; for ropemacs
(require 'pymacs)
(pymacs-load "ropemacs" "rope-")
;;(setq ropemacs-enable-shortcuts nil)
;;(setq ropemacs-local-prefix "C-c C-p")
```
接下来重新启动emacs吧，新建一个test.py
``` py test.py
import sys
sys.
```
在.后面敲下TAB键,MiniBuffer就有提示了

如果不成功的话，大家可以去[python-mode网站](https://launchpad.net/python-mode)

下载下来之后
``` sh
sudo python setup.py install
```

###color theme
[官方网站](http://www.nongnu.org/color-theme/)

[下载链接](http://download.savannah.gnu.org/releases/color-theme/)

- 解压缩之后复制color-theme.el到~/.emacs.d/plugin/theme
- 在theme文件夹下面新建个空文件夹themes
- 新建文件
- color-theme-blackboard.el
- 填充下列内容
``` cl
(defun color-theme-blackboard ()
  "Color theme by JD Huntington, based off the TextMate Blackboard theme, created 2008-11-27"
  (interactive)
  (color-theme-install
   '(color-theme-blackboard
     ((background-color . "#0C1021")
      (background-mode . dark)
      (border-color . "black")
      (cursor-color . "#A7A7A7")
      (foreground-color . "#F8F8F8")
      (mouse-color . "sienna1"))
     (default ((t (:background "#0C1021" :foreground "#F8F8F8"))))
     (blue ((t (:foreground "blue"))))
     (bold ((t (:bold t))))
     (bold-italic ((t (:bold t))))
     (border-glyph ((t (nil))))
     (buffers-tab ((t (:background "#0C1021" :foreground "#F8F8F8"))))
     (font-lock-builtin-face ((t (:foreground "#F8F8F8"))))
     (font-lock-comment-face ((t (:italic t :foreground "#AEAEAE"))))
     (font-lock-constant-face ((t (:foreground "#D8FA3C"))))
     (font-lock-doc-string-face ((t (:foreground "DarkOrange"))))
     (font-lock-function-name-face ((t (:foreground "#FF6400"))))
     (font-lock-keyword-face ((t (:foreground "#FBDE2D"))))
     (font-lock-preprocessor-face ((t (:foreground "Aquamarine"))))
     (font-lock-reference-face ((t (:foreground "SlateBlue"))))
 
     (font-lock-regexp-grouping-backslash ((t (:foreground "#E9C062"))))
     (font-lock-regexp-grouping-construct ((t (:foreground "red"))))
 
     (font-lock-string-face ((t (:foreground "#61CE3C"))))
     (font-lock-type-face ((t (:foreground "#8DA6CE"))))
     (font-lock-variable-name-face ((t (:foreground "#FF6400"))))
     (font-lock-warning-face ((t (:bold t :foreground "Pink"))))
     (gui-element ((t (:background "#D4D0C8" :foreground "black"))))
     (region ((t (:background "#253B76"))))
     (mode-line ((t (:background "grey75" :foreground "black"))))
     (highlight ((t (:background "#222222"))))
     (highline-face ((t (:background "SeaGreen"))))
     (italic ((t (nil))))
     (left-margin ((t (nil))))
     (text-cursor ((t (:background "yellow" :foreground "black"))))
     (toolbar ((t (nil))))
     (underline ((nil (:underline nil))))
     (zmacs-region ((t (:background "snow" :foreground "ble")))))))
```
**保存退出，更新.emacs**
``` cl
;; for the theme
(add-to-list 'load-path "~/.emacs.d/plugin/theme/")
(require 'color-theme)
(color-theme-initialize)
(load-file "~/.emacs.d/plugin/theme/color-theme-blackboard.el")
(color-theme-blackboard)
```
现在配色应该跟TextMate的颜色相近了

当然了，我这里还有个更好的选择

[solarized配色](http://ethanschoonover.com/solarized)

- 这个配色支持很多平台
- 我们就来看emacs的吧

[git repo](https://github.com/sellout/emacs-color-theme-solarized)

### for emacs24
- 不用管color theme的，直接注释掉之前的操作好了
- 然后增加以下操作

1. 下载好zip包
2. 解压到~/.emacs.d/plugin/下面
3. 更新.emacs (这里是可以选择dark或者light的)
``` cl
(add-to-list 'custom-theme-load-path "~/.emacs.d/plugin/sellout-emacs-color-theme-solarized")
(load-theme 'solarized-light t)
```
### for emacs  inferior to 24
- 需要color theme
- 解压到~/.emacs.d/plugin/下面
- 更新.emacs
``` cl
(add-to-list 'load-path "~/.emacs.d/plugin/sellout-emacs-color-theme-solarized")
(require 'color-theme-solarized)
(color-theme-solarized)
```
接下来看下语法监测吧
``` sh
sudo easy_install pyflakes
```
之后更新.emacs
``` cl
;; for PyFlakes
(when (load "flymake" t)
  (defun flymake-pyflakes-init ()
    (let* ((temp-file (flymake-init-create-temp-buffer-copy
               'flymake-create-temp-inplace))
       (local-file (file-relative-name
            temp-file
            (file-name-directory buffer-file-name))))
      (list "pyflakes"  (list local-file))))
  (add-to-list 'flymake-allowed-file-name-masks
        '("\\.py\\'" flymake-pyflakes-init)))
```
**使用M-x flymake-mode RET进入**

最后说下行号的开启吧
``` cl
(global-linum-mode t)
```
至于调试我就放弃了
