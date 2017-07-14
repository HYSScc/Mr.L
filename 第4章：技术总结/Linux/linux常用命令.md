* 查看当前系统信息

```
方法一: 
# cat   /etc/issue
Ubuntu 10.04 LTS \n \l

方法二：
# sudo lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 11.04
Release:        11.04
Codename:       natty
```

* 用户与用户组操作

```
1. 在在命令行输入id，为查看该账户的用户名及用户所属组等信息
# id
uid=1001(lhy) gid=1001(lhy) groups=1001(lhy),0(root)

# id -gn  //查看当前用户的所属组
lhy
```

* 查看进程列表并过滤

```
eg: ps -ef | grep nginx
```

* netstat 查看端口占用

```
　eg: netstat -anp | grep :80

安装netstat: yum install -y net-tools
```

* wget 拉取网页

```
eg: wget www.baidu.com
安装wget：yum install -y wget
```

* curl 访问站点

```
eg: curl -v https://local.keyumall.com
```

* echo向文件写入内容

```
1. 覆盖文件内容：echo "Raspberry" > test.txt
2. 追加文件内容：echo "Intel Galileo" >> test.txt
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



