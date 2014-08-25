title: bash进阶
date: 2012-04-03 00:51
tags: bash
---
###grep高亮关键词
``` sh
alias grep='grep --color=always'
```
- 接下来，cat的话，大家应该比较熟悉
- 但是cat出来的东西是没有语法高亮的，这点让我们很不爽的
- 首先，确定你有python的setup-tools
- 没有的自己去搞，mac自带
``` sh
sudo easy_install Pygments
```
添加bash_profile
``` sh
alias xcat='pygmentize -f console'
```
相关的资料可以去[传送门](http://pygments.org/)

接下来
- 我们经常会遇到这么一个问题
- 一个tab和另一个tab之间的命令经常丢失，互相不能共享让我们很不爽
- 这个其实就是并发的问题，很简单，举个例子

1. new tab #1，读出10条历史命令
2. 执行了5条命令
3. new tab #2，由于tab#1没有关闭，所以tab#2读到10条历史命令
4. 在tab#2执行10条命令
5. 关闭tab#1，15条命令用覆盖的方式写回文件
6. 关闭tab#2，20条命令覆盖文件

- 解决的办法呢
- 那就添加bash_profile
``` sh
shopt -s histappend
```
- 这样写回就是用`追加`的方式了
- 但是这样之后关闭tab#1的时候才会追加,这样并不能满足我们的需求
- bash每执行完一条命令，都要显示一个新的提示符，而在显示提示符的同时，会执行保存在环境变量`PROMPT_COMMAND`里面的命令
- 而通常它执行的是显示当前路径的命令`update_terminal_cwd;`
- 所以解决办法就是通过history -a写回，并修改`PROMPT_COMMAND`
``` sh
PROMPT_COMMAND="history -a; $PROMPT_COMMAND"
```
- 接下来
- 我们平时会遇到这样的问题，每次搜索历史命令都要通过
``` sh
history | grep xxx
```
- 之类的命令来完成
- 我们希望能自动补全搜索，其实通过更改bash的mode也可以做到
``` sh
set -o vi
```
- 但我是emacs党，这个怎么办呢？
- 在HOME目录下面新建.inputrc文件
```
"\e[A": history-search-backward
"\e[B": history-search-forward
```
- 然后通过输入一部分的命令，然后通过方向键来上下查询历史命令
- 接下来
- 修改history的文件大小和条数
``` sh
export HISTFILESIZE=1000000000
export HISTSIZE=1000000
```
忽略重复的命令
``` sh
export HISTCONTROL=ignoredups
```
