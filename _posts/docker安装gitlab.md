title: docker安装gitlab
date: 2014-08-25 10:49:11
tags:
- docker
- gitlab
---
``` bash 准备EPEL
wget http://ftp.riken.jp/Linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -Uvh epel-release-6-8.noarch.rpm
sudo yum update -y
```

``` bash 安装docker
sudo yum install -y docker-io
usermod -a -G docker your_user_name
```

``` bash 准备container
docker pull sameersbn/gitlab:7.2.0
docker pull sameersbn/postgresql:latest
docker pull sameersbn/redis:latest
```

``` bash 创建挂载文件夹
mkdir -p opt/gitlab/data
mkdir -p opt/postgresql/data
```

``` bash 启动postgresql，注意更改对应的挂载目录
docker run --name=postgresql -d \
-v /home/git/opt/postgresql/data:/var/lib/postgresql \
sameersbn/postgresql:latest
```

``` bash 查看postgresql密码
docker logs postgresql
```

``` bash 获取postgresql IP信息
docker inspect postgresql
```

IP地址是返回的json数据里面NetworkSettings.IPAddress

``` bash 进入postgresql，使用之前获取的密码和IP
docker run -it --rm sameersbn/postgresql:latest psql -U postgres -h IP_ADDRESS
```

``` sql 创建用户，自定义密码
CREATE ROLE gitlab with LOGIN CREATEDB PASSWORD 'password';
CREATE DATABASE gitlabhq_production;
GRANT ALL PRIVILEGES ON DATABASE gitlabhq_production to gitlab;
\q
```

``` bash 启动redis container
docker run --name=redis -d sameersbn/redis:latest
```

``` bash 安装gitlab，根据你的挂载位置设置，之前设置的数据库密码
docker run --name='gitlab' -it --rm \
--link redis:redisio \
-v /home/git/opt/gitlab/data:/home/git/data \
-p 10022:22 -p 10080:80 \
-e 'GITLAB_PORT=10080' \
-e 'GITLAB_SSH_PORT=10022' \
--link postgresql:postgresql \
-e 'DB_USER=gitlab' -e 'DB_PASS=password' \
-e 'DB_NAME=gitlabhq_production' \
-e 'GITLAB_EMAIL=gitlab@example.com' \
-e 'SMTP_DOMAIN=www.example.com' \
-e 'SMTP_HOST=smtp.mandrillapp.com' \
-e 'SMTP_USER=username' \
-e 'SMTP_PASS=API_TOEKN' \
-e 'GITLAB_BACKUPS=daily' \
-e 'GITLAB_HOST=example.gitlab.com' \
sameersbn/gitlab:7.2.0 app:rake gitlab:setup
```

``` bash 启动gitlab
docker run --name='gitlab' -d \
[OPTIONS] above
sameersbn/gitlab:7.2.0
```

``` bash 备份
docker run --name='gitlab' -it --rm \
[OPTIONS] above
sameersbn/gitlab:7.2.0 app:rake gitlab:backup:create
```

``` bash 恢复
docker run --name='gitlab' -it --rm \
[OPTIONS] above
sameersbn/gitlab:7.2.0 app:rake gitlab:backup:restore
```