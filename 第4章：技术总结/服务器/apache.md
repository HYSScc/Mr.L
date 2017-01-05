Ubuntu 14.04 安装 Apache: [https://cnbin.github.io/blog/2015/07/03/ubuntu-14-dot-04-an-zhuang-apache/](https://cnbin.github.io/blog/2015/07/03/ubuntu-14-dot-04-an-zhuang-apache/)

ubuntu完全卸载apache2:[http://blog.csdn.net/guojing880208/article/details/6592772](http://blog.csdn.net/guojing880208/article/details/6592772)

UBUNTU 下完全卸载 APACHE2: [https://blog.byneil.com/ubuntu-%E4%B8%8B%E5%AE%8C%E5%85%A8%E5%8D%B8%E8%BD%BD-apache2/](https://blog.byneil.com/ubuntu-%E4%B8%8B%E5%AE%8C%E5%85%A8%E5%8D%B8%E8%BD%BD-apache2/)

解决apache启动错误 AH00558: httpd: Could not reliably determine...: [http://blog.csdn.net/moqiang02/article/details/19644727](http://blog.csdn.net/moqiang02/article/details/19644727)

Ubuntu下Apache服务器的配置：[http://www.ezlippi.com/blog/2016/01/apache-configuration-in-ubuntu.html](http://www.ezlippi.com/blog/2016/01/apache-configuration-in-ubuntu.html)

---

**卸载Apache2**

Ubuntu自带 apache2 有时候很讨厌。 有时候需要先卸载了再装别的。

这里找了一个完全卸载的方法:

```
sudo apt-get autoremove apache2 -y
sudo apt-get remove apache* -y
sudo apt-get --purge remove apache-common -y
sudo apt-get --purge remove apache -y
sudo find /etc -name "*apache*" |xargs  rm -rf 
sudo rm -rf /var/www
sudo rm -rf /etc/libapache2-mod-jk
dpkg -l |grep apache2|awk '{print $2}'|xargs dpkg -P
```

---

**Apache2的配置**

```
cd /etc/apache2
```

这个目录下有许多纯文本文件和子目录，基本作用如下：

* apache2.conf:这是服务器的主要配置文件，几乎所有的配置都通过这个文件来完成，但是为了简洁推荐使用单独的指定的文件来配置不同的模块。
* ports.conf:这个文件用来指定虚拟主机监听的端口号，如果你配置了SSL的时候要检查这个文件是否正确。
* conf.d/:这个目录用来控制Apache的一些特殊配置，比如SSL配置。
* sites-available/:这个目录包括所有不同web站点的虚拟主机文件，不同的请求对应不同的内容，这些都是已有的，并不是正在使用的。
* sites-enabled/:这个目录包含正在使用的虚拟主机的定义，通常只包含到sites-available目录下文件的符号链接。
* mods-\[enabled,available\]/:和上面的类似，只不过这里面包含的是可用的模块。

从Apache的配置目录结构可以知道，它并不是通过单一的文件来配置，贰拾通过模块化来把整个系统拆分成不同的功能，从而能够动态地增加和修改功能。

## 深入Apache2.conf文件内容 {#深入Apache2-conf文件内容}

文件主要分成三部分，全局配置、默认服务器配置和虚拟主机配置，在Ubuntu系统下，这个文件主要负责全局配置，默认服务器和虚拟主机可以通过Include语句来处理。

Include语句允许Apache读取其他配置文件的内容到当前位置，结果就是Apache启动的时候动态生成一个配置文件，如果拉到文件底部会看到很多Include语句，比如ports.conf等。

### 全局配置 {#全局配置}

#### Timeout {#Timeout}

这个参数默认设置为300，意思是服务器有300s来处理每个请求。

#### KeepAlive {#KeepAlive}

如果设置为On，将允许同个客户端每个连接一直保持来处理多个请求\(HTTP长连接\)

#### MaxKeepAliveRequests {#MaxKeepAliveRequests}

这个参数用来设置每个连接最多能处理多少个单独的请求

#### KeepAliveTimeout {#KeepAliveTimeout}

这个参数设置下一个请求来之前来等待多久，超过这个时间自动关闭这个connection。

