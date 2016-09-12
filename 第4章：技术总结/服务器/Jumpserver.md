官网: [http:\/\/www.jumpserver.org\/](http://www.jumpserver.org/)

Github: [https:\/\/github.com\/jumpserver\/jumpserver](https://github.com/jumpserver/jumpserver)

Wiki文档: [https:\/\/github.com\/jumpserver\/jumpserver\/wiki](https://github.com/jumpserver/jumpserver/wiki)

## 安装Jumpserver\(参考Github Wiki－**Debian\/Ubuntu**\):

##### **适用于版本：v0.3.1-2**

1. 安装mysql数据库，创建库

##### **1. 更新软件源**

> apt-get update
> 
> 注：Debian 8.2 需要更换更新源,注释掉官方源，可以添加国内源，在update

##### **2. 安装git**

> apt-get -y install git

##### **3. 下载jumpserver**

> cd \/opt
> 
> git clone [https:\/\/github.com\/jumpserver\/jumpserver.git](https://github.com/jumpserver/jumpserver.git)
> 
> 注：不要安装在\/root、\/home 等目录下，以免权限问题

##### **4. 执行安装脚本**

> cd jumpserver\/install
> 
> python install.py

**注:**

```
    1. 最小化安装环境可能没有装python，请执行 apt-get install python

    2. 安装过程中要求输入数据库密码时，直接回车就行

    3. 完成安装后，请访问web，继续查看后续文档

    4. 如果启动失败，请返回上层目录，手动运行 ./service.sh restart启动

    5. 默认账号密码 admin 5Lov@wife
```

## 问题：

1. 字符集不兼容的错误
  `mysql> drop database jumpserver;`
  `mysql> create database jumpserver default charset='utf8';`
  `mysql>grant all on jumpserver.* to 'jumpserver'@'127.0.0.1' identified by '5Lov@wife';`

