**Docker — 从入门到实践: **https:\/\/yeasy.gitbooks.io\/docker\_practice\/content\/

**Docker 学习笔记 By 枯木: **http:\/\/opskumu.github.io\/docker.html

**Docker 中文指南：**http:\/\/www.widuu.com\/docker\/index.html

---

## 1. 查看docker信息（version、info）

> **\[plain\]** [view plain](http://blog.csdn.net/we_shell/article/details/38368137# "view plain")[copy](http://blog.csdn.net/we_shell/article/details/38368137# "copy")
> 
> \# 查看docker版本
> $docker version
> 
> \# 显示docker系统的信息
> $docker info

## 2. 对image的操作（search、pull、images、rmi、history）

> \# 检索image
> $docker search image\_name
> 
> \# 下载image
> $docker pull image\_name
> 
> \# 列出镜像列表; -a, --all=false Show all images; --no-trunc=false Don't truncate output; -q, --quiet=false Only show numeric IDs
> $docker images
> 
> \# 删除一个或者多个镜像; -f, --force=false Force; --no-prune=false Do not delete untagged parents
> $docker rmi image\_name
> 
> \# 显示一个镜像的历史; --no-trunc=false Don't truncate output; -q, --quiet=false Only show numeric IDs
> $docker history image\_name



