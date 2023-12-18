---
layout: post
title: "Build a jekyll local environment"
description: ""
date: 2020-11-21
tags: []
comments: true
---

之前在电脑上搭建一个本地环境预览blog内容，但换了电脑后，好久没写blog，逐渐就忘了怎么弄，今天想起来决定记录下。

使用Docker搭建jekyll运行环境，需要在Mac上先安装Docker软件，这里省略。

1.构建jekyll镜像
Dockerfile
```text
FROM ruby:2.5.0-alpine3.7

MAINTAINER coolboy1353@163.com

RUN apk update --no-cache \
    && apk add --no-cache --virtual .build-deps build-base \
    && apk add --no-cache git openssh \
    && gem install jekyll jekyll-paginate \
    && apk del -f .build-deps

WORKDIR /opt/blog/
```

执行命令
``` bash
$ docker build -f Dockerfile -t "jekyll" .
```

2.使用docker-compose file管理
docker-compse.local.yml
```
version: '2'
services:
  blog:
    container_name: jekyll
    image: latelan/jekyll:v2.0
    restart: always
    volumes:
        - /Users/latelan/work/github.com/zero:/opt/blog
    ports:
        - 4000:4000
    command: ['jekyll', 's', '-w', '-H', '0.0.0.0']
```
注意将blog目录挂载到/opt/blog，这里blog目录是/Users/latelan/work/github.com/zero

启动服务
```bash
$ docker-compose -f docker-compose.local.yml start
# 后续可使用
$ docker-compose -f docker-compose.local.yml up
```

其他命令记录
```bash
# 新建一篇blog
$ cd /path/to/blog-dir
$ rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]]
```

在浏览器里访问 ```http://127.0.0.1:4000```

![jekyll_local_env](/assets/img/jekyll_local_env.png)

