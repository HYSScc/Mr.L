免密码登陆: ssh -i ./ssh\_keys/aliyun.key intorobot@192.168.1.23

---

ssh-agent用法

# ssh-agent {#ssh-agent}

一直不知道啥是ssh-agent，今晚看了几篇文章，终于领悟到了。

## 一般的ssh过程 {#一般的ssh过程}

## 其实第二步做完以后基本上就不输密码登陆。那需要ssh-agent 以及ssh-add有什么用呢？ {#其实第二步做完以后基本上就不输密码登陆那需要ssh-agent-以及ssh-add有什么用呢}

其实ssh-keygen的时候，可以输密码，也可以不输密码，刚才那种就是不输密码的情况，那么如果你输了密码，ssh 登陆的时候还是会提示你将私钥的密码输入的\(不需要输入服务器的密码\)。而ssh-agent能避免这种反复输入私钥的烦恼。  
用法很简单

## mac上面还有个叫keychain Access 的东西，是管理密码的软件，也是通过ssh-add来实现ssh免密码登陆。 {#mac上面还有个叫keychain-access-的东西是管理密码的软件也是通过ssh-add来实现ssh免密码登陆}



### 参考:

**ssh-keygen 中文手册: **[http:\/\/www.jinbuguo.com\/openssh\/ssh-keygen.html](http://www.jinbuguo.com/openssh/ssh-keygen.html)

**ssh-keygen 的 详解:** [http:\/\/blog.csdn.net\/wh\_19910525\/article\/details\/7433164](http://blog.csdn.net/wh_19910525/article/details/7433164)

