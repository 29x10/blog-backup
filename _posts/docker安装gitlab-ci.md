title: docker安装gitlab-ci
date: 2014-09-01 08:51:28
tags:
- docker
- gitlab
- gitlab-ci
---

<!-- more -->

##变化
没有DNS，因为考虑到通讯的问题，所有server_name均改为IP


``` sh docker pull
docker pull sameersbn/gitlab-ci:5.0.1
```

``` sh 创建文件夹
mkdir -p opt/gitlab-ci/data
mkdir -p opt/gitlab-ci-runner
```

``` sql gitlab-ci建表
CREATE ROLE gitlab_ci with LOGIN CREATEDB PASSWORD 'password';
CREATE DATABASE gitlab_ci_production;
GRANT ALL PRIVILEGES ON DATABASE gitlab_ci_production to gitlab_ci;
```

``` sh 初始化gitlab-ci数据库
docker run --name=gitlab-ci -it --rm \
-p 10090:80 \
-e 'GITLAB_URL=http://x.x.x.x:10080' \
-e 'GITLAB_CI_PORT=10090' \
-e 'GITLAB_CI_HOS=x.x.x.x' \
-e 'GITLAB_CI_SUPPORT=support@localhost' \
--link postgresql:postgresql \
--link redis:redisio \
-v /home/git/opt/gitlab-ci/data:/home/gitlab_ci/data \
-e 'DB_USER=gitlab_ci' -e 'DB_PASS=password' \
-e 'DB_NAME=gitlab_ci_production' \
-e 'GITLAB_CI_EMAIL=gitlab-ci@example.com' \
-e 'SMTP_DOMAIN=www.example' \
-e 'SMTP_HOST=smtp.mandrillapp.com' \
-e 'SMTP_USER=username' \
-e 'SMTP_PASS=API_TOKEN' \
sameersbn/gitlab-ci:5.0.1 app:rake db:setup
```

``` sh 启动gitlab-ci
docker run --name=gitlab-ci -d \
[options above]
sameersbn/gitlab-ci:5.0.1
```

``` sh 安装最简单的runner
docker run --name gitlab-ci-runner -it --rm \
-v /home/git/opt/gitlab-ci-runner:/home/gitlab_ci_runner/data \
sameersbn/gitlab-ci-runner:5.0.0 app:setup
```

``` sh 注册runner
docker run --name gitlab-ci-runner -d \
-v /home/git/opt/gitlab-ci-runner:/home/gitlab_ci_runner/data \
sameersbn/gitlab-ci-runner:5.0.0
```