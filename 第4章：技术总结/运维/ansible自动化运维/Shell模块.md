#### Shell

功能：执行的命令中有管道或者变量，就需要使用shell

`[root@node1 ~]# ansible-doc -s shell`

`- name: Execute commands in` `nodes.`

`action: shell`

`chdir # 执行之前，先cd到指定目录在执行命令`

`creates # 一个文件名，当这个文件存在，则该命令不执行`

`executable # 切换shell来执行命令，需要使用命令的绝对路径`

`free_form= # 执行的命令`

`removes # 一个文件名，这个文件不存在，则该命令不执行`

