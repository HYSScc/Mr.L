**Ubuntu 服务器版 Iptables 基本设置指南: **http:\/\/wiki.ubuntu.org.cn\/IptablesHowTo

> 对每一个报文，iptables依次测试每一条规则，看报文与规则是否相匹配。一旦找到一条匹配的规则，就根据此规则中指定的行动，对报文进行处置，而对后面的规则不再进行测试。因此，如果我们在规则表的末尾添加一条规则，让iptables丢弃所有报文，但由于有了前面几条规则，ssh和web的正常通信不会受到影响。

