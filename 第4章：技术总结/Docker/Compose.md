Docker Compose将所管理的容器分为三层，工程（project），服务（service）以及容器（contaienr）。Docker Compose运行的目录下的所有文件（docker-compose.yml, extends文件或环境变量文件等）组成一个工程，若无特殊指定工程名即为当前目录名。一个工程当中可包含多个服务，每个服务中定义了容器运行的镜像，参数，依赖。一个服务当中可包括多个容器实例，Docker Compose并没有解决负载均衡的问题，因此需要借助其他工具实现服务发现及负载均衡。

Docker Compose的工程配置文件默认为docker-compose.yml，可通过环境变量COMPOSE\_FILE或-f参数自定义配置文件，其定义了多个有依赖关系的服务及每个服务运行的容器。

---

1. **Docker学习笔记\#4: **http:\/\/www.jianshu.com\/p\/784bdffcc469






