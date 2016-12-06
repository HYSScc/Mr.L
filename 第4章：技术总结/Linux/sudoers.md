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





