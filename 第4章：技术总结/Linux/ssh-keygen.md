免密码登陆: ssh -i ./ssh\_keys/aliyun.key intorobot@192.168.1.23

---

ssh-agent用法

# ssh-agent {#ssh-agent}

一直不知道啥是ssh-agent，今晚看了几篇文章，终于领悟到了。

一般的ssh过程

其实第二步做完以后基本上就不输密码登陆。那需要ssh-agent 以及ssh-add有什么用呢？

其实ssh-keygen的时候，可以输密码，也可以不输密码，刚才那种就是不输密码的情况，那么如果你输了密码，ssh 登陆的时候还是会提示你将私钥的密码输入的\(不需要输入服务器的密码\)。而ssh-agent能避免这种反复输入私钥的烦恼。  
用法很简单

1. 先查看ssh-agent ID

   `eval ssh-agent`


1. 将key添加到ssh-agent中  
   `ssh-add aliyun.key`

> ---
>
> ### step1. 开启 ssh-agent {#articleHeader0}
>
> $ eval`ssh-agent`  
> Agent pid XXX
>
> ### step2. 添加私钥 {#articleHeader1}
>
> $ ssh-add ~/.ssh/id\_rsa （如果生成密钥时是使用的默认的，那么就是这个了，如果不是的话就写你的私钥地址吧）
>
> ### step3. 告诉ssh 允许 ssh-agent 转发 {#articleHeader2}
>
> * 修改全局：
>
> ```
> $ echo "ForwardAgent yes" >> /etc/ssh/ssh_config
> ```
>
> * 修改个人
>
> ```
> $ touch ~/.ssh/config
> $ vim ~/.ssh/config
> Host *
> 　　ForwardAgent yes
> ```
>
> ### step4. 修改每台服务器的 ssh 配置文件，让它们都对 ssh-agent 进行转发 {#articleHeader3}
>
> 到每台服务器上去按 step3 -&gt; 全局，做一下。
>
> ---
>
> 在密钥对生成以后，我们需要将客户端上的公钥复制到SSH服务端或者主机，来创建对客户端的信任关系。运行以下命令复制客户端的公钥到服务端。
>
> ```
> $ ssh-copy-id user@ip_address
> eg: $ ssh-copy-id -i aliyun.key.pub root@192.168.189.28 -p 2727
> ```
>
> 在公钥上传之后，我们现在可以禁用通过密码登陆SSH的方式了。为此，我们需要通过以下命令用文本编辑器打开/etc/ssh/ssh\_config。
>
> ```
> $ sudo nano /etc/ssh/sshd_config
> ```

mac上面还有个叫keychain Access 的东西，是管理密码的软件，也是通过ssh-add来实现ssh免密码登陆。

### 参考:

**ssh-keygen 中文手册: **[http://www.jinbuguo.com/openssh/ssh-keygen.html](http://www.jinbuguo.com/openssh/ssh-keygen.html)

**ssh-keygen 的 详解:** [http://blog.csdn.net/wh\_19910525/article/details/7433164](http://blog.csdn.net/wh_19910525/article/details/7433164)

**ssh-agent 使用指南: **[https://segmentfault.com/a/1190000002449006](https://segmentfault.com/a/1190000002449006)

**如何设置 Ubuntu 14.04 的 SSH 无密码登录: **[http://www.linuxidc.com/Linux/2015-04/115825.htm](http://www.linuxidc.com/Linux/2015-04/115825.htm)

