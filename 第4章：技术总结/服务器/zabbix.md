官网：[http:\/\/www.zabbix.com\/](http://www.zabbix.com/)

详解zabbix安装部署\(Server端篇\)：[http:\/\/blog.chinaunix.net\/uid-25266990-id-3380929.html](http://blog.chinaunix.net/uid-25266990-id-3380929.html)

zabbix监控系统客户端安装: [http:\/\/blog.chinaunix.net\/uid-25266990-id-3387002.html](http://blog.chinaunix.net/uid-25266990-id-3387002.html)

# For Debian / Ubuntu



## Supported versions



- Debian 7 (codename: wheezy)

- Debian 8 (codename: jessie)

- Ubuntu 14.04 LTS (codename: trusty)



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

- zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uroot -p 26554422

```

OR

参考 https://www.zabbix.com/documentation/3.2/manual/appendix/install/db_scripts

下载sql文件 http://www.zabbix.com/developers.php

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

 DBPassword=zabbix

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


- After database is installed and zabbix_server.conf file is configured, you may start Zabbix server process.

```

 >> service zabbix-server start

```

- As frontend configuration is done, you need to restart Apache web server.

```

 >> service apache2 restart

```



### 6. Install Agent

```

 sudo apt-get -y install zabbix-agent

//Now agent is ready to be started by:

 sudo service zabbix-agent start

```
