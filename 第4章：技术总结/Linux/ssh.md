SSH 密钥对总是成双出现的，一把公钥，一把私钥。公钥可以自由的放在您所需要连接的 SSH 服务器上，而私钥必须稳妥的保管好。  
所谓”公钥登录”，原理很简单，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录 shell，不再要求密码。这样子，我们即可保证了整个登录过程的安全，也不会受到中间人攻击。

如何使用：  
1. 客户端创建公钥/私钥密码对  
2. 客户端将公钥发给服务端，服务端将公钥内容插入 ~/.ssh/authorized\_keys 中

参考  
[SSH keys](https://wiki.archlinux.org/index.php/SSH_keys_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29)

Tips  
1. 用 ssh/ssh2 登陆时，可以使用 -v 打印调试信息，一般登陆时都有一个顺序，例如先尝试使用 public-key 登陆，如果 public-key 登陆不了，再使用 password 登陆。  
2. authorized\_keys 的文件权限一定是 700/600/400  
3. 要查看客户端和服务端 ssh 版本是否一致，如果一个是 ssh\(通常[Linux](http://lib.csdn.net/base/linux)上默认安装 OpenSSH, 使用的是 ssh 协议，而不是 ssh2\), 一个是 ssh2, 会存在一些不兼容的问题，处理会非常蛋疼。（[ssh 和 ssh2 公钥登陆](http://blog.chinaunix.net/uid-26517277-id-4055228.html)）





---

---

[SSH 公钥免密码登陆: ](http://blog.csdn.net/duyiwuer2009/article/details/50959219)http://blog.csdn.net/duyiwuer2009/article/details/50959219

