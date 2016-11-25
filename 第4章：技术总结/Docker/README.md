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

## 3. 启动容器（run）

docker容器可以理解为在沙盒中运行的进程。这个沙盒包含了该进程运行所必须的资源，包括文件系统、系统类库、shell 环境等等。但这个沙盒默认是不会运行任何程序的。你需要在沙盒中运行一个进程来启动某一个容器。这个进程是该容器的唯一进程，所以当该进程结束的时候，容器也会完全的停止。

> \# 在容器中运行"echo"命令，输出"hello word"
> $docker run image\_name echo "hello word"
> 
> \# 交互式进入容器中
> $docker run -i -t image\_name \/bin\/bash
> 
> \# 在容器中安装新的程序
> $docker run image\_name apt-get install -y app\_name

Note： 在执行apt-get 命令的时候，要带上-y参数。如果不指定-y参数的话，apt-get命令会进入交互模式，需要用户输入命令来进行确认，但在docker环境中是无法响应这种交互的。apt-get 命令执行完毕之后，容器就会停止，但对容器的改动不会丢失。

## 4. 查看容器（ps）

> \# 列出当前所有正在运行的container
> $docker ps
> \# 列出所有的container
> $docker ps -a
> \# 列出最近一次启动的container
> $docker ps -l

## 5. 保存对容器的修改（commit）

当你对某一个容器做了修改之后（通过在容器中运行某一个命令），可以把对容器的修改保存下来，这样下次可以从保存后的最新状态运行该容器。

> \# 保存对容器的修改; -a, --author="" Author; -m, --message="" Commit message
> $docker commit ID new\_image\_name

