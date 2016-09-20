公司项目: http:\/\/ram-lab.com\/molmc\_deployment.git

# **Step 1. Set up the environment on Mac**

* ### **Install Ansible**


#### **    Brew Install**

```
可以采用[Homebrew](http://brew.sh/)进行安装：
```

> brew install ansible \# 安装Ansible
> 
> brew install --upgrade ansible \# 以后可更新版本

#### **    Pip Install**

```
还可采用Python的[pip](https://pip.pypa.io/en/stable/installing/)包管理工具安装：
```

> sudo pip install ansible \# 安装Ansible
> 
> sudo pip install --upgrade ansible \# 以后可更新版本

* ### **Install VirtualBox if not have one**


> brew install Caskroom\/cask\/virtualbox

或在[VirtualBox官网下载](https://www.virtualbox.org/wiki/Downloads)进行安装。

* ### **Install Vagrant if not have one**


> brew install vagrant

或在[Vagrant官网下载](https://www.vagrantup.com/downloads.html)进行安装。

* ### **Vagrant up base on existing **`Vagrantfile`


> 

验证登录虚拟机成功后退出:

> git clone https:\/\/github.com\/Waterstrong\/ansible-workshop.git
> 
> git checkout step1
> 
> cd ansible-workshop\/vagrant
> 
> vagrant up

验证登录虚拟机成功后退出:

> vagrant ssh
> 
> exit

* ### **Test Ansible Connection**


> cd ..
> 
> ansible -i inventory all -m ping

若连接成功返回:

> 192.168.33.100 \| SUCCESS =&gt; {
> 
> "changed": false,
> 
> "ping": "pong"
> 
> }

#### **Unreachable Solution**

如果连接不成功返回:

> 192.168.33.100 \| UNREACHABLE! =&gt; {
> 
> "changed": false,
> 
> "msg": "Failed to connect to the host via ssh.",
> 
> "unreachable": true
> 
> }

可能原因是之前已经在`~/.ssh/known_hosts`中有相同的记录，可以通过ssh命令确认:

> ssh -i vagrant\/.vagrant\/machines\/default\/virtualbox\/private\_key vagrant@192.168.33.100

如果确实报错:

> @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
> 
> @ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @
> 
> @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
> 
> IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
> 
> Someone could be eavesdropping on you right now \(man-in-the-middle attack\)!
> 
> It is also possible that a host key has just been changed.
> 
> The fingerprint for the ECDSA key sent by the remote host is
> 
> SHA256:JIdGdnPGRJcOZd1ZMiisaPesCr3I0\/o00agtrOGNYYA.
> 
> Please contact your system administrator.
> 
> Add correct host key in \/Users\/sqlin\/.ssh\/known\_hosts to get rid of this message.
> 
> Offending ECDSA key in \/Users\/sqlin\/.ssh\/known\_hosts:50
> 
> ECDSA host key for 192.168.33.100 has changed and you have requested strict checking.
> 
> Host key verification failed.

可通过执行以下命令解决:

> ssh-keygen -R 192.168.33.100

或者可直接修改known\_hosts文件，找到该记录并删除:

> sudo vim ~\/.ssh\/known\_hosts \# 找到192.168.33.100记录并删除行后保存

* ### **Environment Ready**

  环境搭建完成，准备工作结束，关闭虚拟机:

  > cd vagrant
  > 
  > vagrant halt


# **Step 2. Inventory Practice**

当前工作目录为`ansible-workshop`，演示使用Inventory文件来指定受控资源列表。

#### **配置虚拟机Host2**

现在可以再加入一台虚拟机，随后会在inventory中进行配置

> mkdir vagrant2
> 
> cd vagrant2
> 
> vagrant init ubuntu\/trusty64

修改Vagrantfile并加入以下配置：

> config.vm.network "private\_network", ip: "192.168.33.101"
> 
> config.vm.provider "virtualbox" do \|vb\|
> 
> vb.name = "ansible-workshop-host2"
> 
> end

启动第二台虚拟机后再回到上一级目录：

> vagrant up
> 
> cd ..

#### **配置Inventory加入新Host2**

创建名为`hosts`的文件，配置虚拟机的Host和Group：

> \[ubuntu\]
> 
> 192.168.33.100 ansible\_ssh\_user=vagrant ansible\_ssh\_private\_key\_file=vagrant\/.vagrant\/machines\/default\/virtualbox\/private\_key
> 
> \[ubuntu2\]
> 
> 192.168.33.101 ansible\_ssh\_user=vagrant ansible\_ssh\_private\_key\_file=vagrant2\/.vagrant\/machines\/default\/virtualbox\/private\_key
> 
> \[myserver:children\]
> 
> ubuntu
> 
> ubunt2

#### **测试是否ping得通**

测试一下应该两台都可以正常访问：

> ansible -i hosts myserver -m ping

可能需要多次输入`yes`回车确认加入key fingerprint，当然也可在ansible.cfg中配置参数关闭提示。当连接成功结果为：

> 192.168.33.100 \| SUCCESS =&gt; {
> 
> "changed": false,
> 
> "ping": "pong"
> 
> }
> 
> 192.168.33.101 \| SUCCESS =&gt; {
> 
> "changed": false,
> 
> "ping": "pong"
> 
> }

也可以单独ping某台虚拟机：

> ansible -i hosts ubuntu2 -m ping

## Q&A:

1. ansible-playbook 中如何使用root权限?

  eg: 需要在远程主机中"git clone https:\/\/github.com\/Waterstrong\/ansible-workshop.git", 但是远程主机需要sudo权限才能执行git操作，此时操作"ansible-playbook -i hosts setup\_server.yml -vvv" 不能克隆成功

2. 群组变量
  语法：\[&lt;group name&gt;:vars\] 在inventory中指定群组变量，如下：

  ```
  [all:vars]
      ntp_server=ntp.centos.com

  [production]
      test1
      test2
      test3
  [production:vars]
      db_primary_port=22

  [groupservers]
      web1.test.com
      web2.test.com
      [groupservers:vars]
      ntp_server=ntp.test.com
      admin_user=tom
  ```

3.  主机变量
  针对单主机的特性化要求，通过内置变量实现，如下：
  ```

  ```


## 参考：

培训文档: [ansible文档](/assets/doc/ansible.html)

视频教程: [http:\/\/edu.51cto.com\/index.php?do=lession&id=42065](http://edu.51cto.com/index.php?do=lession&id=42065)

ansible\/ansible:** **[https:\/\/github.com\/ansible\/ansible](https://github.com/ansible/ansible)

Ansible中文权威指南:** **[http:\/\/www.ansible.com.cn\/index.html](http://www.ansible.com.cn/index.html)

Ansible\_UI: [https:\/\/github.com\/alaxli\/ansible\_ui](https://github.com/alaxli/ansible_ui)

AnsibleUI2:** **[https:\/\/github.com\/alaxli\/AnsibleUI2](https://github.com/alaxli/AnsibleUI2)

Jenkins+Ansible+Gitlab自动化部署三剑客: [http:\/\/www.showerlee.com\/archives\/1880](http://www.showerlee.com/archives/1880)

一步一步用jenkins，ansible，supervisor打造一个web构建发布系统: [http:\/\/hengyunabc.github.io\/deploy-system-build-with-jenkins-ansible-supervisor\/](http://hengyunabc.github.io/deploy-system-build-with-jenkins-ansible-supervisor/)

**Ansible基础篇: **[http:\/\/blog.waterstrong.me\/ansible-basic\/](http://blog.waterstrong.me/ansible-basic/)

**Ansible实践篇: **[http:\/\/blog.waterstrong.me\/ansible-practice\/](http://blog.waterstrong.me/ansible-practice/)

**解决mac osx下pip安装ipython权限的问题: **[http:\/\/xiaorui.cc\/2016\/03\/27\/%E8%A7%A3%E5%86%B3mac-osx%E4%B8%8Bpip%E5%AE%89%E8%A3%85ipython%E6%9D%83%E9%99%90%E7%9A%84%E9%97%AE%E9%A2%98\/\#ds-thread](http://xiaorui.cc/2016/03/27/%E8%A7%A3%E5%86%B3mac-osx%E4%B8%8Bpip%E5%AE%89%E8%A3%85ipython%E6%9D%83%E9%99%90%E7%9A%84%E9%97%AE%E9%A2%98/#ds-thread)

Ansible 自动化运维工具之inventory和常用模块介绍: [http:\/\/www.voidcn.com\/blog\/linuxg\/article\/p-5978444.html](http://www.voidcn.com/blog/linuxg/article/p-5978444.html)

