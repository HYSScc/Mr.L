#### Command

功能：命令模块，默认模块，用于在远程主机执行命令，缺点：运行的命令中无法使用变量，管道。

`[root@node1 ~]# ansible-doc -s command`

`- name: 在远程节点执行命令`

`action: command`

`chdir # 在执行命令之前，先切换到该目录`

`creates # 一个文件名，当这个文件存在，则该命令不执行`

`executable # 切换shell来执行命令，需要使用命令的绝对路径`

`free_form= #要执行的Linux指令，一般使用Ansible的-a参数代替。`

`removes #一个文件名，这个文件不存在，则该命令不执行`

