* 数据库登录
  `mysql -uroot -p`
* 检查MySQL服务器占用端口
  `netstat -nlt|grep 3306`
* 检查MySQL服务器系统进程
  `ps -aux|grep mysql`
* 查看数据库的字符集编码
  `show variables like '%char%';`
*  $ echo -n 'hello'\|md5sum\|cut -d ' ' -f1



