公司项目: http:\/\/ram-lab.com\/molmc\_deployment.git

## 安装：

### **Install Ansible**

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

### **Install VirtualBox if not have one**

> brew install Caskroom\/cask\/virtualbox

或在[VirtualBox官网下载](https://www.virtualbox.org/wiki/Downloads)进行安装。

### **Install Vagrant if not have one**

> brew install vagrant

或在[Vagrant官网下载](https://www.vagrantup.com/downloads.html)进行安装。

### **Vagrant up base on existing **`Vagrantfile`



验证登录虚拟机成功后退出:

\| 1

2 \| vagrant ssh

exit \|
\| --- \| --- \|

---

## 参考：

培训文档: [ansible文档](/assets/doc/ansible.html)

ansible\/ansible:** **[https:\/\/github.com\/ansible\/ansible](https://github.com/ansible/ansible)

Ansible中文权威指南:** **[http:\/\/www.ansible.com.cn\/index.html](http://www.ansible.com.cn/index.html)

Ansible\_UI: [https:\/\/github.com\/alaxli\/ansible\_ui](https://github.com/alaxli/ansible_ui)

AnsibleUI2:** **[https:\/\/github.com\/alaxli\/AnsibleUI2](https://github.com/alaxli/AnsibleUI2)

Jenkins+Ansible+Gitlab自动化部署三剑客: [http:\/\/www.showerlee.com\/archives\/1880](http://www.showerlee.com/archives/1880)

一步一步用jenkins，ansible，supervisor打造一个web构建发布系统: [http:\/\/hengyunabc.github.io\/deploy-system-build-with-jenkins-ansible-supervisor\/](http://hengyunabc.github.io/deploy-system-build-with-jenkins-ansible-supervisor/)

**Ansible基础篇: **[http:\/\/blog.waterstrong.me\/ansible-basic\/](http://blog.waterstrong.me/ansible-basic/)

**Ansible实践篇: **[http:\/\/blog.waterstrong.me\/ansible-practice\/](http://blog.waterstrong.me/ansible-practice/)

**[解决mac osx下pip安装ipython权限的问题: ](http://xiaorui.cc/2016/03/27/%e8%a7%a3%e5%86%b3mac-osx%e4%b8%8bpip%e5%ae%89%e8%a3%85ipython%e6%9d%83%e9%99%90%e7%9a%84%e9%97%ae%e9%a2%98/)**http:\/\/xiaorui.cc\/2016\/03\/27\/%E8%A7%A3%E5%86%B3mac-osx%E4%B8%8Bpip%E5%AE%89%E8%A3%85ipython%E6%9D%83%E9%99%90%E7%9A%84%E9%97%AE%E9%A2%98\/\#ds-thread

