title: bash_prompt配置
date: 2012-04-03 00:10
tags: bash
---
<!-- more -->
**首先就看下最简单的PS1环境变量吧，我的设置效果如下**

![bash](http://i1192.photobucket.com/albums/aa325/kongpo0412/ScreenShot2012-03-14at73718PM.png)

**你可以看到当前时间和当前目录，前面的鸟文可以无视了**

``` sh
export PS1='\[\033[01;34m\]\u\[\033[01;31m\]  \[\033[01;35m\]\h\[\033[01;32m\] \@\n\w \$\[\033[00m\]'
```
首先，prompt不打印的东西要以`\[`和`\[`包起来

以`\033[01;34m`为例

- 先说颜色吧，以esc为开始，也就是`\033`
- `[`右边有三个参数
- 第一个是普通效果(00)还是粗体效果(01)
- 第二个是前景色
- 30=black 31=red 32=green 33=yellow 34=blue 35=magenta 36=cyan 37=white
- 第三个是背景色
- 40=black 41=red 42=green 43=yellow 44=blue 45=magenta 46=cyan 47=white
- 当然了，这里我们不需要设置背景色，自己喜欢折腾的不管

大致的前景背景色预览如下

![color preview](http://i1192.photobucket.com/albums/aa325/kongpo0412/ScreenShot2012-03-14at75650PM.png)

- 然后呢，当prompt结束的时候我们就需要reset了
- `\033[00m`, `00m`就是使用默认的前景色跟背景色了，字体普通
- 那上面那段代码的颜色部分大家也应该理解了

下面我们来看如何拿到自己想要的提示

```
\a  The ASCII bell character (you can also type \007)
\d  Date in "Wed Sep 06" format
\e  ASCII escape character (you can also type \033)
\h  First part of hostname (such as "mybox")
\H  Full hostname (such as "mybox.mydomain.com")
\j  The number of processes you've suspended in this shell by hitting ^Z
\l  The name of the shell's terminal device (such as "ttyp4")
\n  Newline
\r  Carriage return
\s  The name of the shell executable (such as "bash")
\t  Time in 24-hour format (such as "23:01:01")
\T  Time in 12-hour format (such as "11:01:01")
\@  Time in 12-hour format with am/pm
\u  Your username
\v  Version of bash (such as 2.04)
\V  Bash version, including patchlevel
\w  Current working directory (such as "/home/drobbins")
\W  The "basename" of the current working directory (such as "drobbins")
\!  Current command's position in the history buffer
\#  Command number (this will count up at each prompt, as long as you type something)
\$  If you are not root, inserts a "$"; if you are root, you get a "#"
\xxx    Inserts an ASCII character based on three-digit number xxx (replace unused digits with zeros, such as "\007")
\\  A backslash
\[  This sequence should appear before a sequence of characters that don't move the cursor (like color escape sequences). This allows bash to calculate word wrapping correctly.
\]  This sequence should appear after a sequence of non-printing characters.
```
On FreeBSD and Mac OS X, ls shows colors if the `CLICOLOR` environment variable is set or if -G is passed on the command line. The actual colors are configured through the `LSCOLORS` environment variable (built-in defaults are used if this variable is not set)

With GNU ls, e.g. on Linux, ls shows colors if --color is passed on the command line. The actual colors are configured through the `LS_COLORS` environment variable, which can be set with the `dircolors` command (built-in defaults are used if this variable is not set).

所以
``` sh
export CLICOLOR=1
```
- 也就是把ls -G开起来
- 然后我们看下`LSCOLORS`的设置吧
- 直接提供强力工具

[神器在此](http://geoff.greer.fm/lscolors/)

- 自己调吧
- 下面提供下我的
``` sh
export LSCOLORS=gxfxaxdxcxegedabagacad
```
- 然后讲下大概什么意思吧
- 颜色的配置如下
```
a = black
b = red
c = green
d = brown
e = blue
f = magenta
g = cyan
h = light gray
x = default
```
然后看下具体我配置的是什么吧
```
DIR
SYM_LINK
SOCKET
PIPE
EXE
BLOCK_SP
CHAR_SP
EXE_SUID
EXE_GUID
DIR_STICKY
DIR_WO_STICKY
```
颜色的话，大写就是粗体，小写就是普通
