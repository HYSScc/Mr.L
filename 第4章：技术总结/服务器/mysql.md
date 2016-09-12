1. 连接远程数据库服务器

  `mysql -h 192.168.0.77 -P 3306 -u intorobot -p26554422`

2. 查看多少个数据库：注意 后面带s

  ```
  #查看多少个数据库
  SHOW DATABASES;
  ```

  ```
  #查看表
  USE blog;
  SHOW TABLES;
  ```

  ```
  #查看表中的列
  SHOW COLUMNS FROM auth_user;
  ```

  `describe 表名` 是 show columns from 表名 的一种快捷方式

  ```
  DESCRIBE auth_user;
  ```


