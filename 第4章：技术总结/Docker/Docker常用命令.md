1. 设置容器的root密码

```
docker run -d -p 0.0.0.0:2222:22 -e ROOT_PASS="mypass" tutum/centos
```

2. 获取容器的ip

    docker inspect `container_name` | grep IPAddress



