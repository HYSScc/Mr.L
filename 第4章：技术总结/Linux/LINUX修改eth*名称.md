这个方法用于解决Ubuntu下更换网卡后，新网卡变更为eth1，并且源网卡的名称eth0，无法给新网卡用的情况。也可以用于为网卡更名。

网卡MAC地址改变之后，在Linux中找到网卡，新的网卡会被识别为eth1或者更为靠后的网卡写入到/etc/udev/rules.d/70-persistent-net.rules这个文件中，修改/etc/udev/rules.d/70-persistent-net.rules这个文件，将eth0的MAC地址修改为改变后的地址就可以。



编辑

/etc/udev/rules.d/70-persistent-net.rules

[root@shangyuan-laptop:/etc/udev/rules.d](mailto:root@shangyuan-laptop:/etc/udev/rules.d)\# more 70-persistent-net.rules

\# This file maintains persistent names for network interfaces.

\# See udev\(7\) for syntax.

\#

\# Entries are automatically added by the 75-persistent-net-generator.rules

\# file; however you are also free to add your own entries.

\# PCI device 0x14e4:0x1713 \(tg3\)

SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?\*", ATTR{address}=="00:1e:ec:0f:79:f

6", ATTR{type}=="1", KERNEL=="eth\*", NAME="eth0"

\# PCI device 0x8086:0x4222 \(iwl3945\)

SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?\*", ATTR{address}=="00:1f:3c:48:70:b

1", ATTR{type}=="1", KERNEL=="wlan\*", NAME="wlan0"

将其中的eth0,改为eth1,保存后重启系统即可

