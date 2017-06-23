* 设置容器的root密码

```
docker run -d -p 0.0.0.0:2222:22 -e ROOT_PASS="mypass" tutum/centos

eg: sudo docker run -it --rm -e ROOT_PASS="lhy123456" weapp0.0.1 sh
```

* 获取容器的ip

```
docker inspect container_name | grep IPAddress
```

* ssh进入容器

```
ssh -p 22 root@172.17.0.1
```

* Docker端口映射设置

> \# 映射一个端口
>
> EXPOSE port1
>
> \# 相应的运行容器使用的命令
>
> docker run -p port1 image
>
> \# 映射多个端口
>
> EXPOSE port1 port2 port3
>
> \# 相应的运行容器使用的命令
>
> docker run -p port1 -p port2 -p port3 image
>
> \# 还可以指定需要映射到宿主机器上的某个端口号
>
> docker run -p host\_port1:port1 -p host\_port2:port2 -p host\_port3:port3 image



