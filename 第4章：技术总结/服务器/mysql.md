1. 连接远程数据库服务器

  `$ mysql -h 192.168.0.77 -P 3306 -u intorobot -p26554422`

2. 查看多少个数据库：注意 后面带s

  ```
  #查看多少个数据库
  mysql> SHOW DATABASES;
  ```

  ```
  #查看表
  mysql> USE blog;   #访问数据库，使用use语句 注意USE，类似QUIT，不需要一个分号。（如果你喜欢，你可以用一个分号终止这样的语句；这无碍）
  mysql> SHOW TABLES;
  ```

  ```
  #查看表中的列
  mysql> SHOW COLUMNS FROM auth_user;
  ```

  `describe 表名` 是 `show columns from 表名` 的一种快捷方式

  ```
  DESCRIBE auth_user;
  ```

3. 创建数据库

  ```
  mysql> CREATE DATABASE 库名;
  ```

  ```
  mysql> USE 库名;
  mysql> CREATE TABLE 表名 (字段名 VARCHAR(20), 字段名 CHAR(1));    
  ```

  eg:
  ```
  #创建表
  use demo;
  create table pet(
          name varchar(20),        #名字
          owner varchar(20),       #主人
          species varchar(20),     #种类
          sex char(1),             #性别
          birth date,              #出生日期
          death date               #死亡日期
  )
  ```


