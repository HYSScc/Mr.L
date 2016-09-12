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

  为了验证你的表是按你期望的方式创建，使用一个DESCRIBE语句：`describe pet;`

4. 删除数据库

  ```
  mysql> DROP DATABASE 库名;
  ```

5.  删除数据表
  ```
  mysql> DROP TABLE 表名；
  ```

6.  将表中记录清空
  ```
  mysql> DELETE FROM 表名;
  ```

7.  创建表\(复杂形式\)
  ```
  #创建customer表：
  create table customers(
      id int not null auto_increment,
      name char(20) not null,
      address char(50) null,
      city char(50) null,
      age int not null,
      love char(50) not null default 'No habbit',
      primary key(id)
  )engine=InnoDB;
  #SELECT last_insert_id();这个函数可以获得返回最后一个auto_increment值.
  #默认值：default 'No habbit',
  #引擎类型，多为engine = InnoDB，如果省略了engine=语句，则使用默认的引擎(MyISAM)
  ```

8.  更改表结构
  ```
  #增加一列
  alter table pet add des char(100) null;
  #删除
  alter table pet drop column des;
  ```

9.  重命名表
  ```
  #重命名表
  rename table pet to animals;
  ```

10.  **添加id字段**
  ```
  #添加id字段
  alter table pet add id int not null
  primary key auto_increment first;
  ```


## 参考：

Mysql 查看、创建、更改 数据库和表：[http:\/\/www.cnblogs.com\/BeginMan\/p\/3249472.html](http://www.cnblogs.com/BeginMan/p/3249472.html)

