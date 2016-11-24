#### Group

`[root@node1 ~]# ansible-doc -s group`

`- name: 添加或删除组`

`action: group`

`gid # 设置组的GID号`

`name= # 管理组的名称`

`state # 指定组状态，默认为创建，设置值为absent为删除`

`system # 设置值为yes，表示为创建系统组`

