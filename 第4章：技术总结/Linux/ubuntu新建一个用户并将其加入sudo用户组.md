$是普通管员，\#是系统管理员，在Ubuntu下，root用户默认是没有密码的，因此也就无法使用（据说是为了安全）。想用root的话，得给root用户设置一个密码：

$ sudo passwd root

然后登录时用户名输入root，再输入密码就行了。

ubuntu建用户最好用adduser，虽然adduser和useradd是一样的在别的linux糸统下，但是我在ubuntu下用useradd时，并没有创建同名的用户主目录。

**1. 例子：adduser user1**

这样他就会自动创建用户主目录，创建用户同名的组。

root[@ubuntu](http://my.oschina.net/u/555627) :~\# sudo adduser db  \# 使用adduser添加一个用户

\[sudo\] password for xx:

输入xx用户的密码，出现如下信息

正在添加用户"db"…

正在添加新组"db" \(1006\)…

正在添加新用户"db" \(1006\) 到组"db"…

创建主目录"/home/db"…

正在从"/etc/skel"复制文件…

输入新的 UNIX 口令：

重新输入新的 UNIX 口令：

两次输入db的初始密码，出现的信息如下

passwd: password updated successfully

Changing the user information for db

Enter the new value, or press ENTER for the default

Full Name \[\]:

Room Number \[\]:

Work Phone \[\]:

Home Phone \[\]:

Other \[\]:

Full Name \[\]:等信息一路回车

这个信息是否正确？ \[Y/n\] y

到此，用户添加成功。

**2. 将这个账户加入sudo 用户组/admin用户组**

sudo usermod -a -G adm db

sudo usermod -a -G sudo db    \# 此后使用此用户就不会出现如下的错误提示了：

wyx is not in the sudoer file...

sudo usermod -a -G sambashare db    \# 加入此用户组使能用户wyx创建共享文件夹的权限。

**3. 赋予这个用户root**

如果需要让此用户有root权限，执行命令：

sudo chmod u+w /etc/sudoers

root[@ubuntu](http://my.oschina.net/u/555627) :~\# sudo vim /etc/sudoers

修改文件如下：

\# User privilege specification

root ALL=\(ALL\) ALL

db ALL=\(ALL\) ALL

保存退出，db用户就拥有了root权限。

sudo chmod u-w /etc/sudoers

不过我觉得有一个root用户就行了，不要随便将一个普通用户变成root用户（具有和root用户一样权限的超级用户）

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

**账户管理相关的其他命令：**

1. 添加一个用户组并指定id为1002

sudo groupadd －g 1002 www

1. 添加一个用户到www组并指定id为1003

sudo useradd wyx -g 1002 -u 1003 -m

1. 修改用户的密码

sudo passwd wyx

1. 删除一个用户

sudo userdel wyx

1. 为该用户添加sudo权限

sudo usermod -a -G adm wyx

sudo usermod -a -G sudo wyx

1. 查看所有用户和用户组：

cat /etc/passwd

cat /etc/group

