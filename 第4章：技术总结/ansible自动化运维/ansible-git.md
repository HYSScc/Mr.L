
**运维自动化之ansible的安装与使用**

 随着服务器数量的增长，我们需要一个批量工具去提高工作效率，之前用的是puppet，ansible的简单，适用让我眼前一亮，决定写一篇ansible从安装到基本配置的文档供新手参阅。

 

**一、安装**

1.安装第三方epel源

centos 6的epel

 \[root@ansible ~\]\# wget http:\/\/dl.fedoraproject.org\/pub\/epel\/6\/x86\_64\/epel-release-6-8.noarch.rpm

 \[root@ansible ~\]\# rpm -ivh epel-release-6-8.noarch.rpm

 centos5版本的安装5的epel（不详细解释）





2.安装ansible

 \[root@ansible ~\]\# yum install ansible -y

 \[root@ansible ~\]\# ll \/etc\/ansible

 -rw-r--r-- 1 root root 8625 Oct 11 18:06 ansible.cfg

 -rw-r--r-- 1 root root 1046 Oct 11 18:06 hosts

 ansibles.cfg是配置文件，hosts是管理主机信息

 

 建议密钥认证

 \[root@ansible ~\]\# ssh-keygen （直接回车即可）

 推送公钥

 \[root@ansible ~\]\# for i in {1..4}; do ssh-copy-id -i 192.168.78.1$i; done 

 \[root@ansible ~\]\# cat \/etc\/ansible\/hosts

 ...

 \[web\]

 192.168.78.11

 192.168.78.12

 \[mysql\]

 192.168.78.13

 192.168.78.14

 ...

 组名以及ip根本自己需求定义





3.定义变量

1）定义主机变量 

 \[root@ansible ansible\]\# cat hosts

 ...

 \[web\]

 192.168.78.11 http\_port=80

 192.168.78.12 http\_port=303

 ...

 

**\*\*主机指定变量，以便后面供palybooks配置使用。定义两台web服务器的apache参数http\_port，可以让两台服务器产生的apache配置文件httpd.conf差异化\*\*** 



2）定义组变量 

 \[root@ansible ansible\]\# cat hosts

 ...

 \[web\]

 192.168.78.11

 192.168.78.12

 \[web:vars\]

 http\_port=80

**\*\*组变量的作用域是覆盖组所有成员，通过定义一个新块，块名由组名+ ":vars"组成。\*\*** 



3）嵌套组

 \[root@ansible ~\]\# cat \/etc\/ansible\/hosts

 ...

 \[web\]

 192.168.78.11

 192.168.78.12

 \[mysql\]

 192.168.78.13

 192.168.78.14

 \[nested：children\]

 web

 mysql

 \[nested：vars\]

 ntp\_server=s1b.time.edu.cn 

 ...

_**\*\***_**嵌套组定义一个新块，块名由组名+":chilren"组成。同是嵌套组也可以定义组变量，作用域是嵌套组里的所有组 **

**嵌套组只能在\/usr\/bin\/ansible-playbook中，在\/usr\/bin\/ansible中不起作用，下面会介绍playbook**_**\*\***_ 



4）分离主机与组特定数据 

**\*\*为了更好的规范定义的主机与组变量，我们实际是不会在hosts里直接写的，将定义的主机名与组变量单独剥离出来放到指定的文件中，将采用YAML格式存放，存放位置规定："\/etc\/ansible\/group\\_vars\/+组名"和"\/etc\/ansible\/host\\_vars\/+主机名"分别存放指定组名或主机名定义的变量，如下：\*\*** 



**\*\*注意目录结构，规范目录名\*\*** 

 \[root@ansible host\_vars\]\# cat 192.168.78.11.yml

 ---

 http\_port: 80

 \[root@ansible group\_vars\]\# cat mysql.yml

 ---

 ntp\_server: s1b.time.edu.cn

 database\_server: 192.168.78.14

 

 \[root@ansible group\_vars\]\# cat web.yml

 ---

 ntp\_server: s1b.time.edu.cn

 http\_prot: 80

**\*\*可以直接调用变量，规范目录名的原因是ansible会自动加载这几个目录下的变量，如果变量不放到这几个目录下，是不能调用的，我们会在后面介绍到不放到特定目录下，playbook如何调用这些变量\*\***



**二、命令参数**



 Usage: ansible &lt;host-pattern&gt; \[options\] 

 Options:

 -m MODULE\\_NAME, --module-name=MODULE\\_NAME 要执行的模块，默认为 command 

 -a MODULE\_ARGS, --args=MODULE\_ARGS 模块的参数 

 -u REMOTE\_USER, --user=REMOTE\_USER ssh 连接的用户名，默认用 root，ansible.cfg 中可以配置

 -k, --ask-pass 提示输入 ssh 登录密码，当使用密码验证登录的时候用 

 -s, --sudo sudo 运行

 -U SUDO\_USER, --sudo-user=SUDO\_USER sudo 到哪个用户，默认为 root

 -K, --ask-sudo-pass 提示输入 sudo 密码，当不是 NOPASSWD 模式时使用

 -B SECONDS, --background=SECONDS run asynchronously, failing after X seconds\(default=N\/A\)

 -P POLL\_INTERVAL, --poll=POLL\_INTERVAL set the poll interval if using

 -B \(default=15\)

 -C, --check 只是测试一下会改变什么内容，不会真正去执行

 -c CONNECTION 连接类型\(default=smart\)

 -f FORKS, --forks=FORKS fork 多少个进程并发处理，默认 5

 -i INVENTORY, --inventory-file=INVENTORY 指定 hosts 文件路径，默认 default =\/etc\/ansible\/hosts

 -l SUBSET, --limit=SUBSET 指定一个 pattern，对&lt;host\_pattern&gt;已经匹配的主机中再过滤一次

 --list-hosts 只打印有哪些主机会执行这个 playbook 文件：不是实际执行该 playbook

 -M MODULE\_PATH, --module-path=MODULE\_PATH 要执行的模块的路径，默认为\/usr\/share\/ansible\/

 -o, --one-line 压缩输出，摘要输出

 --private-key=PRIVATE\_KEY\_FILE 私钥路径

 -T TIMEOUT, --timeout=TIMEOUT ssh 连接超时时间，默认 10 秒

 -t TREE, --tree=TREE 日志输出到该目录，日志文件名会以主机名命名

 -v, --verbose verbose mode \(-vvv for more, -vvvv to enable connection debugging\)



1\)以 xb 身份 ping 所有主机 

 ansible all -m ping -u xb 

2\)用 xb 用户以 root 身份 ping 

 ansible all -m ping -u xb --sudo 

3\)用 xb 用户 sudo 到 bx 用户 ping 

 ansible all -m ping -u xb --sudo --sudo-user bx

**三、常用模块**

copy模块：

 目的：把主控端\/root目录下的a.sh文件拷贝到到指定节点上 

 命令：ansible 10.1.1.113 -m copy -a ‘src=\/root\/a.sh dest=\/tmp\/ owner=root group=root mode=0755‘

file模块：

 目的：更改指定节点上\/tmp\/t.sh的权限为755，属主和属组为root 

 命令：ansible all -m file -a "dest=\/tmp\/t.sh mode=755 owner=root group=root"

cron模块：

 目的：在指定节点上定义一个计划任务，每隔3分钟到主控端更新一次时间 

 命令：ansible all -m cron -a ‘name="custom job" minute=\\*\/3 hour=\\* day=\\* month=\\* weekday=\\* job="\/usr\/sbin\/ntpdate 172.16.254.139"‘

group模块：

 目的：在所有节点上创建一个组名为nolinux，gid为2014的组 

 命令：ansible all -m group -a ‘gid=2014 name=nolinux‘

user模块：

 目的：在指定节点上创建一个用户名为nolinux，组为nolinux的用户 

 命令：ansible 10.1.1.113 -m user -a ‘name=nolinux groups=nolinux state=present‘ 

 删除用户 

 命令：ansible 10.1.1.113 -m user -a ‘name=nolinux state=absent remove=yes‘

yum模块：

 目的：在指定节点上安装 apache 服务 

 命令：ansible all -m yum -a "state=present name=httpd"

 state=latest=&gt;&gt;安装最新版本

service模块：

 目的：启动指定节点上的 httpd 服务，并让其开机自启动 

 命令：ansible 10.1.1.113 -m service -a ‘name=httpd state=restarted enabled=yes‘

script模块：

 目的：在指定节点上执行\/root\/a.sh脚本\(该脚本是在ansible主控端\) 

 命令：ansible 10.1.1.113 -m script -a ‘\/root\/a.sh‘

ping模块：

 目的：检查指定节点机器是否还能连通 

 命令：ansible 10.1.1.113 -m ping

command模块：

 目的：在指定节点上运行hostname命令

 命令：ansible 10.1.1.113 -m command -a ‘hostname‘

raw模块：

 目的：在10.1.1.113节点上运行ifconfig命令

 命令：ansible 10.1.1.113 -m raw-a ‘ifconfig\|eth0‘

get\_url模块：

 目的：将http:\/\/10.1.1.116\/favicon.ico文件下载到指定节点的\/tmp目录下

 命令：ansible 10.1.1.113 -m get\_url -a ‘url=http:\/\/10.1.1.116\/favicon.ico dest=\/tmp‘

stat模块：

 目的：获取远程文件状态信息，包括atime、ctime、mtime、md5、uid、gid等信息

 ansible web -m stat -a ‘path=\/etc\/sysctl.conf‘

synchronize模块：

 目的：将主控方\/root\/a目录推送到指定节点的\/tmp目录下

 命令：ansible 10.1.1.113 -m synchronize -a ‘src=\/root\/a dest=\/tmp\/ compress=yes‘

 执行效果：

 delete=yes 使两边的内容一样（即以推送方为主）

 compress=yes 开启压缩，默认为开启

 --exclude=.git 忽略同步.git结尾的文件

 mode=pull 更改推送模式为拉取模式

 目的：将10.1.1.113节点的\/tmp\/a目录拉取到主控节点的\/root目录下

 命令：ansible 10.1.1.113 -m synchronize -a ‘mode=pull src=\/tmp\/a dest=\/root\/‘

需要更多模块请使用ansible-doc -l查询

**四、playbook配置管理**

1.语法说明

 - hosts： web \#定义操作的对象，可以是主机或组

 vars： \#这个是变量

 http\_port: 80

 max\_clients: 200

 remote\_user: root \#远端的执行权限，可以理解为远程操作的用户名

 \#任务列表，playbook将按定义的配置文件自上而下的执行，定义的主机都将得到相同的任务，但执行的结果不一定保持一致，

 取决于主机的环境及程序包状态。建议每个任务都要定义一个name标签，增强可读性，也便于观察结果输出时了解运行的位置。

 tasks：

 - name: ensure apache is at the latest version

 yum: pkg=httpd state=latest \#利用yum模块来操作

 - name: write the apache

 template: src=\/srv\/httpd.j2 dest=\/etc\/httpd.conf

 \#触发重启服务器

 notify:

 - restart apache

 - name: ensure apache is running

 service: name=httpd state=started

 \#这里的restart apache 和上面的触发是配对的。这就是handlers的作用。相当于tag

 handlers:

 - name: restart apache

 service: name=httpd state=restarted

\#\#\#\# 如果做了相关sudo限制，需要在playbook里面开启sudo \#\#\#\#



 - hosts: web

 remote\_user: xiaobi

 tasks:

 - service: name=httpd state=started

 sudo: yes

\#\#\# 2.playbook实例 \#\#\#

**\*\*创建一个xiaobi的用户，里面引用了一个user的变量，用jinja2模板给赋值进去了。\*\***



 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 user: root

 gather\_facts: false

 vars:

 - user: "xiaobi"

 tasks:

 - name: create {{ user }}

 user: name={{ user }}

 

**\*\*下面多一个service的调用\*\***



 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 user: root

 gather\_facts: false

 vars:

 - user: "xiaobi"

 tasks

 - name: create {{ user }}

 user: name={{ user }}

 - service: name=httpd state=started



**\*\*再加一个copy模块的调用\*\***

 

 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 user: root

 gather\_facts: false

 vars:

 - user: "xiaobi"

 tasks:

 - name: create {{ user }}

 user: name={{ user }}

 - service: name=httpd state=startedc

 - name: Copy file to client

 copy: src=\/tmp\/tiger dest=\/tmp\/tigress

**\*\*注意：使用copy传送文件的时候，有可能会报错：\*\***

 

 afewbug \| FAILED &gt;&gt; {

 "checksum": "4ee72f7427050dcd97068734d35ca7b2c651bc88", 

 "failed": true, 

 "msg": "Aborting, target uses selinux but python bindings \(libselinux-python\) aren‘t installed!"

**\*\*是因为ansible需要libselinux-python包。（被控端需要安装libselinux-python\*\***） 



**\*\*可以在copy前先调用yum模块，安装libselinux-python\*\***



 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 user: root

 gather\_facts: false

 vars:

 - user: "xiaobi"

 tasks:

 - name: create {{ user }}

 user: name={{ user }}

 - service: name=httpd state=started

 - name: ensure libselinux-python is at the latest

 yum: pkg=libselinux-python state=latest

 - name: Copy file to client

 copy: src=\/tmp\/tiger dest=\/tmp\/tigress

 

**\*\*template模板复制，支持jinja2，以变量say命名\*\***



 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 remote\_user: root

 gather\_facts: false

 vars:

 - user: "xiaobi"

 - say: "tiger"

 tasks:

 - name: create {{ user }}

 user: name={{ user }}

 - service: name=httpd state=started

 - name: ensure libselinux-python is at the latest version

 yum: pkg=libselinux-python state=latest

 - name: Copy file to client

 \# copy: src=\/tmp\/tiger dest=\/tmp\/tigress

 template: src=\/tmp\/tiger dest=\/tmp\/{{ say }}

 



**\*\*不只是这样，还可以把刚才的那个say变量传到文件里，其实和saltstack是一样的\*\*** 

 



**\*\*再加一个执行外部命令的模块\*\***



 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 remote\_user: root

 gather\_facts: false

 vars:

 - user: "xiaobi"

 - say: "tiger"

 tasks:

 - name: create {{ user }}

 user: name={{ user }}

 - service: name=httpd state=started

 - name: ensure libselinux-python is at the latest version

 yum: pkg=libselinux-python state=latest

 - name: Copy file to client

 \# copy: src=\/tmp\/tiger dest=\/tmp\/tigress

 template: src=\/tmp\/tiger dest=\/tmp\/{{ say }}

 - name: run this command and ignore the result

 shell: \/usr\/bin\/somecommand \|\| \/bin\/true

 - name: "cmd"

 command: touch \/tmp\/tigress

 \# - name: "cmd"

 \# action: command touch \/tmp\/tigress

 

_\*\*_注释： 

1.\/usr\/bin\/somecommand：执行某些命令 \|\|： 前面失败的话\/bin\/true：返回true。当你执行一些特定的命令，得到的结果是你可能无法预测的，不希望他的结果为false而中断，而是继续执行，这个true就有意义了。类似的判断还有chenge\_when参数 

2.老版本的写法是action: commadn touch 现在基本弃用了，ansible0.8或以上版本可以直接调用shell，command模块，结果是一样的。_\*\*_



 \)



**\*\*小节总结：已经完成的操作会提示绿色ok，没有做过的操作会提示黄色changed。shell一类执行是没有完整的行为判断的。只有结果输出，所有永远为changed\*\*** 



 ansible-playbook user.yml -f 10

_\*\*_ansible在多任务下，推荐使用多进程模式的。其实就是用multiprocess做的多进程池 ！ -f 10 就是limit 10个任务并发。

fork默认是5,如果有5个客户端,那么这5个是同时执行的;如果是6个,那么第6个要等前面5个执行完_\*\*_



\#\#\# 3.变量功能 \#\#\#



1）系统变量 

_\*\*_前言介绍：获取远程主机系统信息==&gt;&gt;Facts。 

facts是一个非常有用的组件，类似于Saltstack的Grains功能，puppet的facter的功能。实现获取远程主机的系统信息，包括主机名、ip地址、操作系统、分区信息、硬件信息等，可以配合playbook实现更加个性化、灵活的功能需求，比如在httpd.conf模板中引用Facts的主机名信息作为ServerName参数的值。通过运行ansible web -m setup可获取Facts信息，下面是一些实例：_\*\*_ 



 ansible web -m setup

**\*\*这里只展示一部分，可以得到很多信息，这些信息是可以作为变量引用的\*\***

 



**\*\*以下请看圈起来的地方对比的看就容易明白了\*\***

 

 

 



2）自定义变量 

**\*\*我们可以通过Facts来获取目标主机的系统信息，当这些信息还不能满足我们的功能需求时，通过编写自己定义的的Facts模块来实现。当然，还有一个更简单的实现方法，就是通过本地Facts来实现。只需在目标设备\/etc\/ansible\/facts.d目录定义JSON、INI或可执行文件的JSON输出，文件扩展名要求使用".fact"，这些文件都可以作为Ansible的本地Facts，看下面例子，在目标主机192.168.78.11定义三个变量，供以后playbook进行引用\*\*** 

**\*\*①！！！注意：此操作是在目标主机，后面会介绍在主控端操作：\*\***



 \[root@192 ~\]\# mkdir \/etc\/ansible\/facts.d -p

 \[root@192 ~\]\# cd \/etc\/ansible\/facts.d

 \[root@192 facts.d\]\# vim icsoc.fact

 \[general\]

 apple=1

 banana=2

 orange=3

**\*\*在主控端运行ansibles 192.168.78.11 -m setup -a ‘filter=ansible\\_local‘\*\***

 

**\*\*注意返回JSON的层次结构，icsoc（文件名前缀）→general（INI的节名）→key:value（INI的键与值），现在我们就可以在我们的模板或playook中通过以下方式进行调用：\*\*** 

**\*\*{{ ansible\\_local.icsoc.general.apple }}\*\*** 



**\*\*②下面来说一切操作都在主控端进行\*\***



**\*\*在主控端创建icsoc\\_new.fact文件，如下：（下面会提到为什么以icsoc\\_new.fact命名而不能使用icsoc-new.fact命名原因）\*\***

 

 \[root@ansible ansible\]\# cat \/etc\/ansible\/icsoc\_new.fact

 \[general\]

 apple=11

 banana=22

 orange=33

**\*\*在user.yml中加入创建目录，以及把主控端的icsoc\_new.fact推送到目标主机\*\***



 \[root@ansible ansible\]\# cat user.yml

 - name: create user

 hosts: web

 remote\_user: root

 gather\_facts: true

 vars:

 - user: "xiaobi"

 - say: "tiger"

 tasks:

 - name: create {{ user }}

 user: name={{ user }}

 - service: name=httpd state=started

 - name: ensure libselinux-python is ata the latest version

 yum: pkg=libselinux-python state=latest

 - name: Copy file to client

 \# copy: src=\/tmp\/tiger dest=\/tmp\/tigress

 template: src=\/tmp\/tiger dest=\/tmp\/{{ say }}

 - name: "cmd"

 command: touch \/tmp\/tigree

 - name: run this command and ignore the result

 shell: \/usr\/bin\/somecommand \|\| \/bin\/true

 - name: "reference facts variable"

 template: src=\/tmp\/bbb dest=\/tmp\/{{ ansible\_default\_ipv4.address }}

 - name: create directory for ansible custom facts

 file: state=directory recurse=yes path=\/etc\/ansible\/facts.d

 - name: install custom impi fact

 copy: src=\/etc\/ansible\/icsoc\_new.fact dest=\/etc\/ansible\/facts.d

 - name: re-read facts after adding custom fact

 setup: filter=ansible\_local

**\*\*运行ansible-playbook user.yml截图太长不截图了，和上面基本一样\*\*** 

此时在目标主机就有了\/etc\/ansible\/facts.d\/icsoc\\_new.fact文件。 

**\*\*运行ansible web -m setup -a ‘filter=ansible\\_local‘\*\***

** \*\*注意返回JSON的层次结构，icsoc\_new（文件名前缀）→general（INI的节名）→key:value（INI的键与值），现在我们就可以在我们的模板或playook中通过以下方式进行调用：\*\***

**\*\*{{ ansible\\_local.icsoc\\_new.general.apple }}\*\*** 

**\*\*!!!注意：变量名的命名规则由字母、数字和下划线组合而成，变量必须以字母开头，如：icsoc\\_new,icsoc5都是可以的，icsoc-new和5icsoc是不可以的，这也是我们创建本地facts的时候不使用icsoc-new.fact命名的原因。\*\*** 



3）注册变量 

**\*\*变量的另一个用途是将一条命令的运行结果保存到变量中，供后面的playbook使用。下面介绍一个简单的示例：（由于截图太长，我们重新写一个简短的user1.yml，方便截图）\*\***



 \[root@ansible ansible\]\# cat user1.yml

 - hosts: web

 remote\_user: root

 tasks:

 - shell: \/usr\/bin\/foo

 register: foo\_result

 ignore\_errors: True

 - shell: touch \/tmp\/kkkk

 when: foo\_result.rc == 5

_\*\*_上述示例注释： 

1.注册了一个foo\\_resul变量，变量值为shell: \/usr\/bin\/foo的运行结果; 

2.ignore\\_errors: True为忽略错误 

3.当变量注册完成后，就可以在后面的playbook中使用了 

4.当条件语句when: foo\\_result.rc == 5成立时，shell: touch \/tmp\/kkkk命令才会执行 

5.foo\\_result.rc为返回\/usr\/bin\/foo的resultcode（返回码）返回"rc=0"的返回码_\*\*_ 

 



**\*\*由于\/usr\/bin\/foo的运行结果返回码是127，所以shell: touch \/tmp\/kkkk命令没有执行，这里显示跳过了。下面我们再修改一下user1.yml把when: foo\\_result.rc == 5改为127，再测试一下\*\***



 \[root@ansible ansible\]\# cat user1.yml

 - hosts: web

 remote\_user: root

 tasks:

 - shell: \/usr\/bin\/foo

 register: foo\_result

 ignore\_errors: True

 - shell: touch \/tmp\/kkkk

 when: foo\_result.rc == 127

 



\#\#\# 4.语句 \#\#\#

\#\#\#\#1）条件语句\#\#\#\#

_\*\*_前言： 

1.有时候一个playbook的结果取决于一个变量，或者取决于上一个任务（task）的执行结果值，在某些情况下，一个变量的值可以依赖于其他变量的值，当然也会影响ansible的执行过程 

2.有时候我们想跳过某些主机的执行步骤，比如符合特定版本的操作系统将不安装某个软件包，或者磁盘空间满了将进行清理的步骤。 

3.这些在ansible中很容易做到，通过when实现，其中将引用jinja2表达式。_\*\*_



 \[root@ansible ansible\]\# cat when.yml

 - name: when

 hosts: web

 remote\_user: root

 gather\_facts: true

 tasks:

 - name: "shutdown Debian flavored systems"

 command: \/sbin\/shutdown -t now

 when: ansible\_os\_family == "Debian"

**\*\*通过变量ansible\_os\_family == "Debian"（操作系统版本名称）是否为Debian，结果将返回BOOL类型值，为True时将执行上一条语句command: \/sbin\/shutdown -t now，为False时该条语句都不会触发。\*\*** 



**\*\*下面再写一个通过判断一条命令执行结果做不同分支的二级处理\*\***



 \[root@ansible ansible\]\# cat when1.yml

 - name: when

 hosts: web

 remote\_user: root

 gather\_facts: true

 tasks:

 - command: \/sbin\/ip a

 register: result

 ignore\_errors: True

 - command: \/bin\/something

 when: result\|failed

 - command: \/bin\/something\_else

 when: result\|success

 - command: \/bin\/still\/something\_else

 when: result\|skipped

 

**\*\*"when： result\|success"的意思为当变量result执行结果为成功状态是，将执行\/bin\/something\_else命令，其他同理。其中success为Ansible内部过滤器方法，返回True代表命令运行成功。\*\***

\#\#\#\# 2）循环语句 \#\#\#\#



 \[root@ansible ansible\]\# cat wheel.yml

 - name: whell

 hosts: web

 remote\_user: root

 gather\_facts: true

 tasks:

 - name: "Batch add user"

 user: name={{ item }} state=present groups=wheel

 with\_items:

 - tiger1

 - tiger2

 

**\*\*以上示例是批量创建系统用户的功能。with\\_items会自动循环执行上面的语句"user: name={{ item }} state=present groups=wheel"，循环的次数为with\\_items的元素个数。这里有2个元素，分别为tiger1、tiger2，会分别替换{{ item }}项，这个示例与下面的示例是等价的：\*\***



 \[root@ansible ansible\]\# cat wheel.yml

 - name: whell

 hosts: web

 remote\_user: root

 gather\_facts: true

 tasks:

 - name: "add user tiger1"

 user: name=tiger1 state=present groups=wheel

 - name: "add user tiger2"

 user: name=tiger2 state=present groups=wheel

**\*\*元素也支持字典的形式，如下：\*\***



 \[root@ansible ansible\]\# cat wheel.yml

 - name: whell

 hosts: web

 remote\_user: root

 gather\_facts: true

 tasks:

 - name: "Batch add user"

 user: name={{ item.name }} state=present groups={{ item.groups }}

 with\_items:

 - { name: ‘tiger1‘,groups: ‘wheel‘ }

 - { name: ‘tiger2‘,groups: ‘root‘ }

**\*\*循环也支持列表（list）的形式，不过是通过with\_flattened语句来实现的，如下：\*\***



 \[root@ansible ansible\]\# cat \/etc\/ansible\/123.yml

 packages\_base:

 - \[ ‘vsftpd‘, ‘vim‘ \]

 packages\_apps:

 - \[\[ ‘mysql‘, ‘httpd‘ \]\]

**\*\*以上定义了两个列表变量，分别是需要安装的软件包名，以便后面进行引用，如下：\*\***

 

 \[root@ansible ansible\]\# cat \/etc\/ansible\/wheel1.yml

 - name: whell

 hosts: web

 vars\_files: \#之前说的在特定的目录下定义变量文件，可以自动加载。如果在自定义路径，我们需要这里指明具体路径

 - \/etc\/ansible\/123.yml

 remote\_user: root

 gather\_facts: true

 tasks:

 - name: "Batch install rpms"

 yum: name={{ item }} state=installed

 with\_flattened: \#通过使用with\_flattened语句循环引用定义好的列表变量

 - packages\_base

 - packages\_apps 

 **\*\*通过使用with\_flattened语句循环引用定义好的列表变量\*\***

\#\#\# 5.handlers与include \#\#\#

**\*\*当多个playbook涉及复用的任务列表时，可以将复用的内容剥离出来，写到独立的文件里，需要的地方include进来即可\*\*** 

**\*\*除了tasks之外，还有一个handlers的命令，handlers是在执行tasks之后服务器发生变化之后可供调用的handler，如下所示：\*\***



 \[root@ansible ansible\]\# cat httpd.yml

 - name: write the httpd config file

 hosts: web

 remote\_user: root

 gather\_facts: true

 tasks:

 - name: write the httpd.conf to client

 template: src=\/httpd.conf.j2 dest=\/etc\/httpd\/conf\/httpd.conf

 notify: \# 如果copy执行完之后\/etc\/httpd\/conf\/httpd.conf文件发送了变化，则执行

 - restart httpd \# 调用handler

 - include: playbook\/tasks\/httpd.yml

 handlers:

 - name: restart httpd \#此处的标识必须和notify一样才可以引起触发

 service: name=httpd state=restarted

**\*\*注意上面使用的- include: playbook\/tasks\/httpd.yml，看一下这个文件的内容，\*\***

 

 \[root@ansible ansible\]\# cat playbook\/tasks\/httpd.yml

 - name: ensure httpd is running

 service: name=httpd state=started

 

 

## 参考：

20分钟教你学会熟练使用ansible： http://www.mamicode.com/info-detail-1428476.html

文档: [http:\/\/docs.ansible.com\/ansible\/git\_module.html\#examples](http://docs.ansible.com/ansible/git_module.html#examples)

