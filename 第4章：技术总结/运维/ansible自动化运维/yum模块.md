#### yum

`[root@node1 ~]# ansible-doc -s yum`

``- name: Manages packages with the `yum' package manager``

`action: yum`

`conf_file # yum的配置文件`

`disable_gpg_check # 关闭gpg_check`

`disablerepo # 不启用某个源`

`enablerepo # 启用某个源`

`name= # 指定要安装的包，如果有多个版本需要指定版本，否则安装最新的包`

``state # 安装(`present')，安装最新版(`latest')，卸载程序包(`absent')``

