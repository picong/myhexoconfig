---
layout: post
section-type: post
title: springboot整合kafka
category: tech
comments: true
tags: [ 'docker' ]
---

### Docker 百度百科: 
>Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的linux或
windows机器上，也可以实现虚拟化。容器时完全使用沙箱机制，相互之间不会有任何接口。

### docker一些常用命令
命令|详情
---:|---:
docker pull|获取image
docker build|创建image
docker images|列出image
docker run|运行container
docker ps|列出container
docker rm|删除container
docker rmi|删除image
docker cp|在host和container之间拷贝文件
docker commit|保存改动为新的image

### dockerfile 介绍
命令|用途
---:|---:
FROM|base image
RUN|执行命令
ADD|添加文件(可以添加远程文件路径)
COPY|拷贝文件
CMD|给容器制定执行的入口
EXPOSE|暴露端口
WOEKDIR|制定路径
MAINTAINER|维护者
ENV|设定环境变量
ENTRYPOINT|容器入口
USER|制定用户
VOLUME|mount point
Dockerfile中的每一行都产生一个新层

### Volume
这条命令是将本地的本目录下的html目录挂载到docker中的nginx的html目录下面，修改宿主机的文件docker中的也变了
```
docker run -p 80:80 -v $PWD/html:/usr/share/nginx/html nginx
```
创建一个本地data挂载到docker中mydata上的容器data_container
```
docker create -v $PWD/data:/var/mydata --name data_container ubuntu
下面这条命令进入docker容器的bash shell里面
docker run -it --volumes-from data_container ubuntu /bin/bash 
```

### Registry
镜像仓库


### docker-compose 常用命令
命令|用途
---:|---:
up|启动服务
stop|停止服务
rm|删除服务中的各个容器
logs|观察各个容器的日志
ps|列出服务器相关的容器
