官方文档: [https:\/\/www.vagrantup.com\/docs\/](https://www.vagrantup.com/docs/)

**ubuntu 安装vagrant过程: **http:\/\/www.mamicode.com\/info-detail-193664.html

使用Vagrant（一）：搭建Vagrant运行环境: [http:\/\/www.ituring.com.cn\/article\/131438](http://www.ituring.com.cn/article/131438?utm_source=tuicool&utm_medium=referral)

---

Vagrant 官方提供了很好的 [Getting Started](https://docs.vagrantup.com/v2/getting-started/project_setup.html)，按照这个文档一步步操作就可以学会怎么用它了。我这里也简单写一下使用步骤。

首先，新建一个目录作为工作目录，例如 `vagrant_project`：

```
$ mkdir vagrant_project
$ cd vagrant_project

```

这时候，必须介绍一下 Boxes 这个概念了。Boxes 是一个后缀为 `box` 的文件，实际上它就是一个包含了虚拟机配置、虚拟机硬盘镜像和 Vagrant 配置的压缩包。之前在虚拟机里安装 Linux，首先需要下载 iso 安装镜像并新建虚拟机，然后修改虚拟机的 cpu、内存、光驱、网络等等硬件配置，再从光驱启动并安装虚拟机，完成后还有装虚拟机增强工具、配置系统、映射端口、映射磁盘目录等好多事要做。有了封装好的 Boxes，我们就可以快速运行所需要的操作系统了。不同 Provider 之间，Boxes 不能混用。以 VirtualBox 为例，下面这样就可以运行一个 `Ubuntu Server 14.04` 虚拟机了：

```
$ vagrant init ubuntu/trusty64
$ vagrant up

```

`ubuntu/trusty64` 是一个公开 Boxes，更多公开 Boxes 可以在[这里](https://atlas.hashicorp.com/boxes/search) 找到。运行上面第一行命令后，Vagrant 会在工作目录下创建 `Vagrantfile` 配置文件，打开它可以看到下面这样一行，作用是配置当前 Vagrant 环境使用哪个 Boxes：

```
config.vm.box = "ubuntu/trusty64"

```

通过 `vagrant up` 命令，Vagrant 会尝试启动这个虚拟机，但此时虚拟机还没创建，于是 Vagrant 需要找到用于创建虚拟机的 Boxes。根据 `config.vm.box` 配置，Vagrant 首先会看看本机是否已经存在 `ubuntu/trusty64` 对应的 Boxes，如果本机没有就去网上下载。一旦下载完成，之后再创建相同 Boxes 的 Vagrant 环境直接从本地加载就可以了。

一个 Boxes 通常有几百 MB，执行上述命令后可以留意一下屏幕输出，查看 Boxes 的下载速度。如果速度很慢，可以复制出具体的 `.box` 文件 URL 并终止这次命令，改用其他工具下载。有了下载好的 Boxes 文件之后，打开 `VagrantFile`，修改 `config.vm.box` 配置为本地文件：

```
BASH# config.vm.box = "ubuntu/trusty64"
# config.vm.box = "http://example.com/virtualbox.box"
config.vm.box = "~/Downloads/virtualbox.box"

```

再执行 `vagrant up` 就会从本地加载 Boxes，稍等片刻就好了。可以看到，`config.vm.box` 配置还可以配置成 URL，提前下载常用的 Boxes 放在内网供大家使用也是一个不错的办法。

Vagrant 环境创建并启动后，可以通过 `vagrant ssh` 进入这个虚拟机环境。通常不需要输入帐号密码就可以登录，登录之后的用户名是 vagrant，密码也是 vagrant。这个帐号执行 sudo 不需要输入密码，因为封装 Boxes 时已经做了相应处理。

然后就可以对虚拟机进行一些常规配置（如修改 apt-get 为国内源），并且安装所需要的软件。这部分内容略过不写。

虚拟机环境配好后，我们希望尽可能在宿主机操作，web 环境代码、配置最好都放在外面。Vagrant 已经考虑到这一点，主机的 Vagrant 工作目录（例如前面的 vagrant\_project）会被自动映射为虚拟机的 `/vagrant` 目录。所以我们只要把虚拟机里的 Nginx 配置文件和 vhost 主目录都放在 `/vagrant` 里即可。

接着我们还要做一件重要的事情：将虚拟机里某些端口映射出来。打开 `VagrantFile`，找到 `config.vm.network` 配置，去掉前面的注释：

```
config.vm.network "forwarded_port", guest: 80, host: 8080

```

这样就可以通过主机的 8080 端口访问虚拟机 80 端口上的服务了。如果还有其他端口需要映射，照这个格式再加一行就可以。

有一点需要注意，虚拟机上的某些服务如 Nginx，如果配置为开机自动启动，并且它的配置文件在 `/vagrant` 目录，很可能启动服务时目录映射还未建好，导致启动失败。这时候需要去掉它的自动启动，并通过 `VagrantFile` 中的以下配置启动它：

```
BASH$script = <<-SCRIPT
sudo service nginx start > /dev/null 2>&1
SCRIPT
config.vm.provision "shell", inline: $script, run: "always"

```

每次修改 `VagrantFile` 之后，需要执行 `vagrant reload` 命令使之生效。其他常用的管理虚拟机的 vagrant 命令还有：

| 命令作用 |  |
| --- | --- |
| vagrant up | 启动本地环境 |
| vagrant halt | 关闭本地环境 |
| vagrant suspend | 暂停本地环境 |
| vagrant resume | 恢复本地环境 |
| vagrant reload | 修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up） |
| vagrant ssh | 通过 ssh 登录本地环境所在虚拟机 |
| vagrant destroy | 彻底移除本地环境 |

实际使用过程中，通过 vagrant suspend\/resume 来快速暂停 \/ 恢复最为方便。

### 封装

进入 Vagrant 工作目录后，通过 `vagrant package` 命令即可将当前环境封装为一个 Boxes，这个 `.box` 文件就可以给其他人用了，例如将它传到内网。其他人这样就可以快速获得这个环境：

```
$ vagrant init http://example.com/virtualbox.box
$ vagrant up

```

但是这样创建好的环境，并不会包含 VagrantFile 中的一些配置。有两种解决方案：1）通过 `vagrant package --vagrantfile` 将 VagrantFile 也打包进去；2）修改你的 VagrantFile，将 `config.vm.box` 配置为 URL，然后把 VagrantFile 共享出去，其他人只需在新建的工作目录中下载这个 VagrantFile，并 `vagrant up` 就可以了。我们实际使用中采用的是第二种方案。

封装好的 Boxes 还可以提交到公共平台给其他人使用，这部分内容请查看[这份文档](https://atlas.hashicorp.com/learn/vagrant)。

我们还可以自己从 iso 安装镜像开始，封装一个属于自己的 Base Boxes，这部分内容请看[这份文档](https://docs.vagrantup.com/v2/boxes/base.html)。

另外，vagrant 还可以与 [docker](https://www.docker.com/) 结合来用。docker 是一个 Linux Container，并不是虚拟机，所以更轻量化。如果宿主机器不是 Linux，vagrant 会自动创建一个 Linux 虚拟机来运行 docker，后续所有 docker 类型的 vagrant 环境都会共用这个虚拟机，这部分内容请查看[这份文档](https://docs.vagrantup.com/v2/docker/basics.html)。

本文先写到这里，大家在使用 Vagrant 中有什么心得欢迎留言交流。

本文链接：[https:\/\/imququ.com\/post\/vagrantup.html](https://imququ.com/post/vagrantup.html "Permalink to 开始使用 Vagrant")，[参与评论 »](https://imququ.com/post/vagrantup.html#comments)

--EOF--

