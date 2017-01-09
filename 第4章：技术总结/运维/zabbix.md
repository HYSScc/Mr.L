zabbix web默认用户名:Admin    密码:zabbix

配置文件目录: /usr/share/zabbix/include

# 配置：

Zabbix完整的监控配置流程可以简单描述为：

**Host groups（主机组）→Hosts（主机）→Applications（监控项组）→Items（监控项）→Triggers（触发器）→Event（事件）→Actions（处理动作）→User groups（用户组）→Users（用户）→Medias（告警方式）→Audit（日志审计）**

# 安装：

## For Debian / Ubuntu

## Supported versions

* Debian 7 \(codename: wheezy\)

* Debian 8 \(codename: jessie\)

* Ubuntu 14.04 LTS \(codename: trusty\)

### 1. Installing repository configuration package

```
wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+trusty_all.deb

 sudo dpkg --install zabbix-release_3.2-1+trusty_all.deb

 sudo apt-get update
```

### 2. Installing packages

```
- apt-get install zabbix-server-mysql zabbix-frontend-php

 // Administrator 'root' password: 26554422
```

### 3. Creating initial database

```
- zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uroot -p26554422
```

OR

参考 [https://www.zabbix.com/documentation/3.2/manual/appendix/install/db\_scripts](https://www.zabbix.com/documentation/3.2/manual/appendix/install/db_scripts)

下载sql文件 [http://www.zabbix.com/developers.php](http://www.zabbix.com/developers.php)

```
shell> mysql -uroot -p<password>

mysql> create database zabbix character set utf8 collate utf8_bin;

mysql> grant all privileges on zabbix.* to zabbix@localhost identified by '26554422';

mysql> quit;

shell> cd database/mysql

shell> mysql -uzabbix -p<password> zabbix < schema.sql

# stop here if you are creating database for Zabbix proxy

shell> mysql -uzabbix -p<password> zabbix < images.sql

shell> mysql -uzabbix -p<password> zabbix < data.sql
```

### 4. Common configuration

##### 1. Database configuration for Zabbix server

```
>> vi /etc/zabbix/zabbix_server.conf



 DBHost=localhost

 DBName=zabbix

 DBUser=zabbix

 DBPassword=26554422
```

##### 2. PHP configuration for Zabbix frontend

> NOTE: /etc/apache2/conf.d/zabbix.conf and /etc/zabbix/apache.conf are the same file.

```
DBHost=localhost

DBName=zabbix

DBUser=zabbix

DBPassword=zabbix
```

### 5. Starting Zabbix server process

* After database is installed and zabbix\_server.conf file is configured, you may start Zabbix server process.

```
>> service zabbix-server start
```

* As frontend configuration is done, you need to restart Apache web server.

```
>> service apache2 restart
```

### 6. Install Agent

```
sudo apt-get -y install zabbix-agent

//Now agent is ready to be started by:

 sudo service zabbix-agent start
```

---



修订版改正了很多错误，但是这个论坛不能修改原文。很麻烦

  


Zabbix 3.0 for Ubuntu 14.04 LTS（aliyun）

  


  


准备工作

  


  


apt-get install gettext

  


apt-get install unzip

  


apt-get install rar

  


一、安装主程序

  


  


代码: 全选

  


wget

[http://repo.zabbix.com/zabbix/3.0/ubunt ... ty\_all.deb](http://repo.zabbix.com/zabbix/3.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.0-1+trusty_all.deb)

  


  


dpkg -i zabbix-release\_3.0-1+trusty\_all.deb

  


apt-get update

  


apt-get install zabbix-server-mysql zabbix-frontend-php\# 装服务器端

  


apt-get install zabbix-agent

  


  


  


二、设置数据库表（假设你设置的mysql的root密码是123456\)

  


  


shell

&gt;

 mysql -uroot -p123456

  


mysql

&gt;

 create database zabbix character set utf8 collate utf8\_bin;

  


mysql

&gt;

 grant all on zabbix.\* to zabbix@localhost identified by 'zabbix';

  


mysql

&gt;

 quit;

  


shell

&gt;

 cd /usr/share/doc/zabbix-server-mysql

  


shell

&gt;

 zcat create.sql.gz \| mysql -uroot -p123456 zabbix

  


  


三、编辑zabbix的设置文件

  


  


\# vi /etc/zabbix/zabbix\_server.conf

  


DBHost=localhost

  


DBName=zabbix

  


DBUser=zabbix

  


DBPassword=zabbix

  


  


四、编辑conf文件，准备安装

  


  


vi /etc/apache2/conf-enabled/zabbix.conf

  


编辑时区

  


php\_value max\_execution\_time 300

  


php\_value memory\_limit 128M

  


php\_value post\_max\_size 16M

  


php\_value upload\_max\_filesize 2M

  


php\_value max\_input\_time 300

  


php\_value always\_populate\_raw\_post\_data -1

  


\# php\_value date.timezone Europe/Riga

  


  


最后一行，去掉＃号，时区改成 Asia/Shanghai

  


  


开始安装

  


先重启服务

  


service zabbix-server start

  


service apache2 restart

  


  


然后浏览器登录：

  


  


[http://yourhost/zabbix](http://yourhost/zabbix)

  


  


数据库帐号是zabbix，密码是你设置的密码：zabbix

  


一路安装。。。web登录帐号是Admin／zabbix，基本ok！

  


  


五、优化设置

  


  


1、启用中文

  


vi /usr/share/zabbix/include/locales.inc.php

  


把zh\_CN后面参数写true

  


  


然后去选择语言吧。

  


  


如果，去选择语言的时候，你发现还是不能选择。。。。

  


提示：

  


You are not able to choose some of the languages, because locales for them are not installed on the web server.

  


是因为你系统里没中文环境

  


那么：设置中文环境

  


第一步，安装中文包：

  


apt-get install language-pack-zh-hant language-pack-zh-hans

  


第二步，配置相关环境变量：

  


vi /etc/environment

  


在文件中增加语言和编码的设置：

  


LANG="zh\_CN.UTF-8"

  


LANGUAGE="zh\_CN:zh:en\_US:en"

  


第三步，重新设置本地配置：

  


dpkg-reconfigure locales

  


  


现在重启apache

&

zabbix\_server两个服务一下，应该可以选了。。

  


  


2、但是我发现翻译的不好，有大神做了更好的翻译

  


参见：https://github.com/echohn/zabbix-zh\_CN

  


先进入

  


cd /usr/share/zabbix/locale/zh\_CN/LC\_MESSAGES目录

  


代码: 全选

  


wget

[https://github.com/echohn/zabbix-zh\_CN/ ... master.zip](https://github.com/echohn/zabbix-zh_CN/archive/master.zip)

  


  


unzip master.zip

  


rm frontend.mo

  


cp zabbix-zh\_CN-master/frontend.mo frontend.mo

  


  


现在重启apache

&

zabbix\_server两个服务

  


service zabbix-server restart

  


service apache2 restart

  


  


3、看图时候，如果有中文，会乱码

  


调整图像里的中文乱码

  


下载雅黑

  


代码: 全选

  


wget

[http://dx.sc.chinaz.com/Files/DownLoad/font2/dd.rar](http://dx.sc.chinaz.com/Files/DownLoad/font2/dd.rar)

  


  


解压缩文件

  


rar x dd.rar

  


cp dd/msyh.ttf msyh.ttf

  


然后修改 vi /usr/share/zabbix/include/defines.inc.php

  


找到

  


define\('ZBX\_GRAPH\_FONT\_NAME', 'graphfont'\); // font file name

  


修改成：

  


define\('ZBX\_GRAPH\_FONT\_NAME', 'msyh'\); // font file name

  


  


重启apache服务即可

  


  


4、重要的mibs库，必须更新，否则snmp监控交换机时，mib会报错。

  


apt-get install snmp-mibs-downloader

  


六、一些提示 tips

  


  


重新启动zabbix－server服务进程

  


\# service zabbix-server restart

  


重新启动zabbix－agent进程

  


\# service zabbix-server restart

  


重启apache进程

  


＃service apache2 restart

  


  


重要目录:

  


log: /var/log/zabbix/zabbix\_server/log和agent.log 排查错误必须

  


conf：/etc/zabbix/\*.conf

  


安装目录：/usr/share/zabbix 重要的include，font .etc

  


根web目录在var/www/html

---

官网：[http://www.zabbix.com/](http://www.zabbix.com/)

[Ubuntu 14.04 x64 安装 Zabbix 3.2.0 Server端+多系统Agent端部署: ](https://www.deamwork.com/archives/zabbix3.orz6)[https://www.deamwork.com/archives/zabbix3.orz6](https://www.deamwork.com/archives/zabbix3.orz6)

**Zabbix企业级分布式监控系统：**[https://github.com/itnihao/zabbix-book](https://github.com/itnihao/zabbix-book)

详解zabbix安装部署\(Server端篇\)：[http://blog.chinaunix.net/uid-25266990-id-3380929.html](http://blog.chinaunix.net/uid-25266990-id-3380929.html)

zabbix监控系统客户端安装: [http://blog.chinaunix.net/uid-25266990-id-3387002.html](http://blog.chinaunix.net/uid-25266990-id-3387002.html)

