#### User

`[root@node1 ~]# ansible-doc -s user`

`- name: 管理用户帐号`

`action: user`

`comment # 用户的描述信息`

`createhome # 是否创建家目录`

``force # 在使用`state=absent'是, 行为与`userdel --force'一致.``

`group # 指定基本组`

`groups` `# 指定附加组，如果指定为('groups=')表示删除所有组`

`home # 指定用户家目录`

`login_class #可以设置用户的登录类 FreeBSD, OpenBSD and NetBSD系统.`

``move_home # 如果设置为`home='时, 试图将用户主目录移动到指定的目录``

`name= # 指定用户名`

`non_unique # 该选项允许改变非唯一的用户ID值`

`password # 指定用户密码`

``remove # 在使用 `state=absent'时, 行为是与 `userdel --remove'一致.``

`shell # 指定默认shell`

`state #设置帐号状态，不指定为创建，指定值为absent表示删除`

`system # 当创建一个用户，设置这个用户是系统用户。这个设置不能更改现有用户。`

`uid #指定用户的uid`

`update_password # 更新用户密码`

