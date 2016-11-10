> 对每一个报文，iptables依次测试每一条规则，看报文与规则是否相匹配。一旦找到一条匹配的规则，就根据此规则中指定的行动，对报文进行处置，而对后面的规则不再进行测试。因此，如果我们在规则表的末尾添加一条规则，让iptables丢弃所有报文，但由于有了前面几条规则，ssh和web的正常通信不会受到影响。
> 
> 机器重启后，iptables中的配置信息会被清空。您可以将这些配置保存下来，让iptables在启动时自动加载，省得每次都得重新输入。`iptables-save`和`iptables-restore`是用来保存和恢复设置的。

## 参考：

**Ubuntu 服务器版 Iptables 基本设置指南: **http:\/\/wiki.ubuntu.org.cn\/IptablesHowTo

**iptables详解：**http:\/\/yijiu.blog.51cto.com\/433846\/1356254

