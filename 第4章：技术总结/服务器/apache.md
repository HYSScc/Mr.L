Ubuntu 14.04 安装 Apache: [https://cnbin.github.io/blog/2015/07/03/ubuntu-14-dot-04-an-zhuang-apache/](https://cnbin.github.io/blog/2015/07/03/ubuntu-14-dot-04-an-zhuang-apache/)

ubuntu完全卸载apache2:[http://blog.csdn.net/guojing880208/article/details/6592772](http://blog.csdn.net/guojing880208/article/details/6592772)

UBUNTU 下完全卸载 APACHE2: [https://blog.byneil.com/ubuntu-%E4%B8%8B%E5%AE%8C%E5%85%A8%E5%8D%B8%E8%BD%BD-apache2/](https://blog.byneil.com/ubuntu-%E4%B8%8B%E5%AE%8C%E5%85%A8%E5%8D%B8%E8%BD%BD-apache2/)

解决apache启动错误 AH00558: httpd: Could not reliably determine...: [http://blog.csdn.net/moqiang02/article/details/19644727](http://blog.csdn.net/moqiang02/article/details/19644727)

---

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



