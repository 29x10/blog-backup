title: heroku转github
date: 2012-04-07 22:22
tags: octopress
---
先说几个问题吧，在墙外还好，速度都很理想，但是墙内的话

1. twitter的bar不能显示,但仍然会去加载，方便人访问还是去掉了展示
2. 人人的分享js，这玩意总是最后去加载,disqus和twitter这两个一个是加载要时间，一个是墙外= =，所以也去掉了
3. heroku今天巨卡无比，我无奈了，您好歹也是ruby的= =

人人的解决方案，因为主要是自己分享自己文章用的

`http://widget.renren.com/dialog/share?charset=UTF-8&resourceUrl=`

迁移到github的问题还是蛮多的
两种选择

1. github_user_name.github.com
2. guthub_user_name.github.com/repo_name

明显第一个舒服，然后兴冲冲的就去了，注册了个新的账号(为了自定义username)然后add ssh key的时候就傻了，原来不能用同一个的

然后就用现在的账号去搞，clone了repo. 一路下去没有问题，然后depoly的时候就傻了，提示找不到某个文件，而这个文件早八百年前就被干掉了，无奈了，伪造了一个，跳过，部署成功，然后我就发现了，post的后缀会无视的repo_name的，然后放弃

之后就去查怎么搞两个github

跟我做吧
``` sh
cd ~/.ssh
rm *
ssh-keygen -t rsa -C "email_prefix_1@email_host_1" -f rsa_file_name_1
ssh-keygen -t rsa -C "email_prefix_2@email_host_2" -f rsa_file_name_2
ssh-add ~/.ssh/rsa_file_name_1
ssh-add ~/.ssh/rsa_file_name_2
touch ~/.ssh/config
```
增加以下内容
```
Host your_host_name_1
HostName github.com
User git
PreferredAuthentications publickey
IdentityFile ~/.ssh/rsa_file_name_1

Host your_host_name_2
HostName github.com
User git
PreferredAuthentications publickey
IdentityFile ~/.ssh/rsa_file_name_2
```
之后你只要
``` sh
ssh -T your_host_name_1
ssh -T your_host_name_2
```
应该都是没有问题的

但是把,之前那个乱七八糟的找不到的提示变的更奇怪了，目录也变了。我瞬间觉得是rake的缓存，但是找不到任何文件可以删除，上网去搜，搜到的命令还都不能用，自己通过网上的命令改改删删试试的，然后发现`rake clean`就是我想要的了，以后大家发现相关问题也可以这么做

然后修改branch的时候记得url的格式是这样的`git@your_hostname(1 or 2 or ...):github_username/repo_name.git`

然后`rake setup_github_pages`的repo同理

今天刷网络的书，slash的主题提交的issue也修正了
