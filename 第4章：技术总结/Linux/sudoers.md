# Ubuntu新添加用户无法sudo问题的解决：修改sudoers

Ubuntu新添加的hadoop用户无法通过sudo执行[命令](http://www.ahlinux.com/start/cmd/)，提示：

hadoop is not in the sudoers file...

解决这个问题只需要切换到其他可以执行sudo的用户，修改/etc/sudoers文件即可，但是这个文件的修改要很小心，它默认的权限为440，所以要改权限，修改完文件再把权限改回去

改权sudoers文件的权限时要注意：

用sudo su -彻底切换到root，否则是不行滴

执行命令如下：

$ sudo su -

\#[chmod](http://www.ahlinux.com/start/cmd/9052.html) a+w /etc/[sudo](http://www.ahlinux.com/start/cmd/2560.html)ers

\# vi /etc/sudoers

\# chmod a-w /etc/sudoers

其中vi /etc/sudoers之后，需要在root ALL\(ALL\) ALL一行之后加上：

hadoop ALL\(ALL\) ALL

保存退出即可，其中hadoop是需要授予sudo执行权限的[用户](http://www.ahlinux.com/start/base/9435.html)名

修改sudoers文件的另外一个方法是网上很多地方都有的：

重启Ubuntu，按Esc或者Shift键进入grub的引导菜单，选择进入recovery mode，进入后选择root登录，便可以修改sudoers文件的权限

如果系统上的所有用户都用不了sudo的话也只好用这种办法了

# 免密码使用sudo和su

转自：[http://www.jianshu.com/p/5d02428f313d](http://www.jianshu.com/p/5d02428f313d)

> 因为最近频繁的使用su root命令，受够了每次都要输入密码，于是网上搜了搜解决方案，还真有解决&gt;  方案，不敢独享，整理分享给大家。  
> 奉上原帖地址：  
> [http://www.cnblogs.com/itech/archive/2009/08/07/1541017.html](http://www.cnblogs.com/itech/archive/2009/08/07/1541017.html)

#### 设置sudo免密码

> sudo是linux系统管理指令，是允许系统管理员让普通用户执行一些或者全部的root命令的一个工具，如halt、reboot、su等等。

* 登录到root用户

* 将用户加入sudoers


```
$ visudo  //或者vi /etc/sudoers
```

移动光标，到一行root ALL=\(ALL\) ALL的下一行，按a，进入append模式，输入

`your_user_name ALL=(ALL) ALL`

然后按Esc，再输入:w保存文件，再:q退出

这样就把自己加入了sudo组，可以使用sudo命令了。

* 默认5分钟后刚才输入的sodo密码过期，下次sudo需要重新输入密码，如果觉得在sudo的时候输入密码麻烦，把刚才的输入换成如下内容即可：

`your_user_name ALL=(ALL) NOPASSWD: ALL`

