#### Script

`[root@node1 ~]# ansible-doc -s script`

`- name: 将本地脚本复制到远程主机并运行之`

`action: script`

`creates # 一个文件名，当这个文件存在，则该命令不执行`

`free_form= # 本地脚本路径`

`removes # 一个文件名，这个文件不存在，则该命令不执行`

