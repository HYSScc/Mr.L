#### Ansible中Roles的使用

**Roles的介绍**

Roles是ansible自1.2版本引入的新特性，用于层次性，结构化地组织playbook，roles能够根据层次型结构自动自动装在变量文件、tasks以及handlers

等。要使用roles只需要在playbook中使用include指令即可。简单来讲，roles就是通过分别将变量、文件、任务、模板及处理器放置于单独的目录中

并可以便捷地include他们的一种机制，角色一般用于主机构建服务的场景中，但也可以是用于构建守护进程等场景中。

**创建roles的步骤**

* 创建以roles命名的目录：

* 在roles目录中分别创建以各角色名称命名的目录，如webservers等：

* 在每个角色命名的目录中分别创建files、handlers、meta、tasks、templates和vars目录：用不到的目录可以创建为空目录，也可以不创建。

* 在playbook文件中，调用各角色


**roles内各目录中可用的文件**

* tasks目录：至少创建一个名为main.yml的文件，其定义了此角色的任务列表：此文件可以使用 include包含其他的位于此目录中的tasks文件：

* files目录：存放由copy或者script等模块调用的文件：

* templates目录：templates模块会自动在此目录中寻找Jinjia2模板文件：

* handlers目录：此目录中应当包含一个main。

* yml文件：用于定义此角色用到的各handler：在handler中使用include包含的其他的handler文件也应该位于此目录中：

* vars目录：应当包含一个main.yml文件，用于定义此角色用到的变量

* meta目录：应当包含一个main.yml文件，用于定义此角色的特殊设定及其依赖关系：ansible 1.3及其以后的版本才支持

* default目录：为当前角色定义默认变量时使用此目录，应该包含一个main.yml文件


roles：

（1）目录名同角色名

（2）目录结构有固定格式：

（3） files：静态文件

（4） templates：Jinjia2 模板文件

（5） tasks：至少有一个main.yml文件，定义tasks：

（6） hangdlers：至少有一个main.yml文件，定义各handlers

（7） vars：至少有一个main.yml文件，定义变量

（8） meta：定义依赖关系等信息

**Roles的使用案例**

下面是一个role的目录结构

```
zengchengpeng@iZ2339ljvmkZ:/etc/ansible/roles/wf_jdk$ tree
.
├── defaults
│   └── main.yml
├── files
│   └── rpm
│       └── jdk-8u66-linux-x64.rpm
├── handlers
├── Readme
└── tasks
    └── main.yml
```

解释说明：

上面这个role是用来安装JDK的，安装包放在files目录中，defaults目录下面的main.yml则是定义默认变量的，文件内容如下：

```
zengchengpeng@iZ2339ljvmkZ:/etc/ansible/roles/wf_jdk$ cat defaults/main.yml 
---
# stable or latest
jdk_path: /etc/ansible/roles/wf_jdk
```

可以看到这个文件用来定义的是jdk\_path这个变量

tasks目录下的main.yml文件内容：

```
zengchengpeng@iZ2339ljvmkZ:/etc/ansible/roles/wf_jdk$ cat tasks/main.yml
```

```
---
- name: create /srv/jdk directory
  file: path=/srv/jdk state=directory mode=0755
- name: sync jdk rpm package
  synchronize: src={{ jdk_path }}/files/rpm/ dest=/srv/jdk/ delete=yes
- name: install jdk
  yum: name={{ item }} state=present disable_gpg_check=yes
  with_items:
    - /srv/jdk/jdk-8u66-linux-x64.rpm
- name: Edit profile JDK conf
  blockinfile:
    dest: /etc/profile
    backup: yes
    marker: "# {mark} jdk config"
    content: |
      JAVA_HOME=/usr/java/jdk1.8.0_66
      CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar
      PATH=$JAVA_HOME/bin:$ANT_HOME/bin:$PATH
      export JAVA_HOME CLASSPATH PATH
```

从上面的tasks中的main.yml文件可以看到安装JDK的一个基本流程，

1、在远程服务器上创建一个\/srv\/jdk的目录

2、将本地的JDK RPM包同步到远程主机上的\/srv\/jdk目录中，如果远程目录中存在这个文件，则先删除再同步

3、用yum模块安装这个RPM包

4、编辑远程主机上的\/etc\/profile文件，在文件中追加以下配置。

```
JAVA_HOME=/usr/java/jdk1.8.0_66
CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar
PATH=$JAVA_HOME/bin:$ANT_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

如何调用Roles

Roles写好之后，需要创建一个playbook文件，然后用ansible-playbook命令去调用这个playbook。所有的playbook可以放在一个目录中，这个目录名开始自己随便定义。例如生产环境的playbook目录名叫workflow。下面是所有的playbook

```
zengchengpeng@iZ2339ljvmkZ:/etc/ansible/workflow$ ls
update_fm.yml              wf_init_iptables.yml      wf_iptables_disabled_port.yml  wf_start_app.yml
wf_add_user.yml            wf_init_kernel.yml        wf_iptables_open_port.yml      wf_stop_app.yml
wf_app_check.yml           wf_init_vim.yml           wf_link_latest_version.yml     wf_web_cdn.yml
wf_check_host.yml          wf_init.yml               wf_logstash.yml                wf_web_check.yml
wf_code_upload.yml         wf_init_zabbix_agent.yml  wf_pgdb.yml                    wf_web_code_upload.yml
wf_del_user.yml            wf_install_jdk.yml        wf_redis.yml                   wf_zabbix_add_template.yml
wf_init_command_audit.yml  wf_install_nginx.yml      wf_reload_nginx.yml            wf_zabbix_create_host.yml
wf_init_common.yml         wf_install_python27.yml   wf_slb_in.yml                  wf_zabbix_maintenance.yml
wf_init_deploy_user.yml    wf_install_tomcat.yml     wf_slb_out.yml
```

Playbook的内容如下：

```
zengchengpeng@iZ2339ljvmkZ:/etc/ansible/workflow$ cat wf_install_jdk.yml
```

```
---
  - hosts: '{{ HOST if HOST|length()>0 }}'
    remote_user: root
    roles:
      - yaegashi.blockinfile
      - wf_jdk
```

注意：上面安装JDK的playbook中多了一个

```
yaegashi.blockinfile
```

这个是因为生产环境的ansible是1.9版本，不支持blockinfile模块。所以单独写了一个roles，来解决这个问题。

**总结：**

上面这个安装JDK的roles是一个比较简单的roles，使用的都是一些比较常用的模块，如果看不明白，则需要熟悉下ansible的相关模块的使用。

Ansible官方文档有所有模块的介绍，下面是官网链接

http://docs.ansible.com/ansible/modules\_by\_category.html

