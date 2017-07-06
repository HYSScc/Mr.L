* 查看进程列表并过滤

```
eg: ps -ef | grep nginx
```

* netstat 查看端口占用

```
　eg: netstat -anp | grep :80

安装netstat: yum install net-tools
```

* wget 拉取网页

```
eg: wget www.baidu.com
安装wget：yum install -y wget
```

* 数据库登录
  ```
  mysql -uroot -p
  ```
* 检查MySQL服务器占用端口
  ```
  netstat -nlt|grep 3306
  ```
* 检查MySQL服务器系统进程
  ```
  ps -aux|grep mysql
  ```
* 查看数据库的字符集编码
  ```
  show variables like '%char%';
  ```
* shell脚本内计算MD5值  
  `$ echo -n 'hello'|md5sum|cut -d ' ' -f1`

  > 命令解释：
  >
  > md5sum: 显示或检查 MD5\(128-bit\) 校验和,若没有文件选项，或者文件处为"-"，则从标准输入读取。
  >
  > echo -n : 不打印换行符。
  >
  > cut:  cut用来从标准输入或文本文件中剪切列或域。剪切文本可以将之粘贴到一个文本文件。
  >
  > -d 指定与空格和tab键不同的域分隔符。-f1 表示第一个域。参考这里。
  >
  > 转自: [http://blog.chinaunix.net/uid-26522150-id-3039950.html](http://blog.chinaunix.net/uid-26522150-id-3039950.html)

* 当前文件目录路径  
  `PWD=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd)`



