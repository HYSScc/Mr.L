#### Service

`[root@node1 ~]# ansible-doc -s service`

`- name: 管理服务`

`action: service`

`arguments # 向服务传递的命令行参数`

`enabled # 设置服务开机自动启动，参数为yes|no`

`name= # 控制服务的名称`

`pattern # 定义一个模式，如果通过status指令来查看服务的状态时，没有响应，就会通过ps指令在进程中根据该模式进行查找，如果匹配到，则认为该服务依然在运行`

`runlevel # 设置服务自启动级别`

`sleep` `# 如果执行了restarted，则在stop和start之间沉睡几秒钟`

``state # 启动`started' 关闭`stopped' 重新启动 `restarted' 重载 `reloaded'``

