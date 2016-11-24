收集远程主机的facts

每个被管理节点在接收并运行管理命令之前，会将自己主机相关信息，如操作系统版本、IP地址等报告给远程的ansbile主机；

示例：查看172.16.4.101主机状态信息，输出内容很多，但是通俗易懂，这里不做过多说明



`[root@node1 ~]# ansible 172.16.4.101 -m setup`

`172.16.4.101 | success >> {`

`"ansible_facts": {`

`"ansible_all_ipv4_addresses": [`

`"172.16.4.101"`

`],`

`"ansible_all_ipv6_addresses": [`

`"fe80::20c:29ff:fef1:ddb2"`

`],`

`"ansible_architecture": "x86_64",`

`"ansible_bios_date": "05/20/2014",`

`"ansible_bios_version": "6.00",`

`"ansible_cmdline": {`

`"KEYBOARDTYPE": "pc",`

`"KEYTABLE": "us",`

`"LANG": "en_US.UTF-8",`

`"SYSFONT": "latarcyrheb-sun16",`

`"crashkernel": "auto",`

`"quiet": true,`

`"rd_LVM_LV": "vg0/root",`

`"rd_NO_DM": true,`

`"rd_NO_LUKS": true,`

`"rd_NO_MD": true,`

`"rhgb": true,`

`"ro": true,`

`"root": "/dev/mapper/vg0-root"`

`},`

`"ansible_date_time": {`

`"date": "2015-06-28",`

`"day": "28",`

`"epoch": "1435476945",`

`"hour": "15",`

`"iso8601": "2015-06-28T07:35:45Z",`

`"iso8601_micro": "2015-06-28T07:35:45.390263Z",`

`"minute": "35",`

`"month": "06",`

`"second": "45",`

`"time": "15:35:45",`

`"tz": "CST",`

`"tz_offset": "+0800",`

`"weekday": "Sunday",`

`"year": "2015"`

`},`

`"ansible_default_ipv4": {`

`"address": "172.16.4.101",`

`"alias": "eth0",`

`"gateway": "172.16.0.1",`

`"interface": "eth0",`

`"macaddress": "00:0c:29:f1:dd:b2",`

`"mtu": 1500,`

`"netmask": "255.255.0.0",`

`"network": "172.16.0.0",`

`"type": "ether"`

`},`

`"ansible_default_ipv6": {},`

`"ansible_devices": {`

`"sda": {`

`"holders": [],`

`"host": "SCSI storage controller: LSI Logic / Symbios Logic 53c1030 PCI-X Fusion-MPT Dual Ultra320 SCSI (rev 01)",`

`"model": "VMware Virtual S",`

`"partitions": {`

`"sda1": {`

`"sectors": "409600",`

`"sectorsize": 512,`

`"size": "200.00 MB",`

`"start": "2048"`

`},`

`"sda2": {`

`"sectors": "125829120",`

`"sectorsize": 512,`

`"size": "60.00 GB",`

`"start": "411648"`

`}`

`},`

`"removable": "0",`

`"rotational": "1",`

`"scheduler_mode": "cfq",`

`"sectors": "377487360",`

`"sectorsize": "512",`

`"size": "180.00 GB",`

`"support_discard": "0",`

`"vendor": "VMware,"`

`},`

`"sr0": {`

`"holders": [],`

`"host": "IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)",`

`"model": "VMware IDE CDR10",`

`"partitions": {},`

`"removable": "1",`

`"rotational": "1",`

`"scheduler_mode": "cfq",`

`"sectors": "2097151",`

`"sectorsize": "512",`

`"size": "1024.00 MB",`

`"support_discard": "0",`

`"vendor": "NECVMWar"`

`}`

`},`

`"ansible_distribution": "CentOS",`

`"ansible_distribution_major_version": "6",`

`"ansible_distribution_release": "Final",`

`"ansible_distribution_version": "6.6",`

`"ansible_domain": "",`

`"ansible_env": {`

`"CVS_RSH": "ssh",`

`"G_BROKEN_FILENAMES": "1",`

`"HOME": "/root",`

`"LANG": "C",`

`"LC_CTYPE": "C",`

`"LESSOPEN": "||/usr/bin/lesspipe.sh %s",`

`"LOGNAME": "root",`

`"MAIL": "/var/mail/root",`

`"PATH": "/usr/lib64/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin",`

`"PWD": "/root",`

`"QTDIR": "/usr/lib64/qt-3.3",`

`"QTINC": "/usr/lib64/qt-3.3/include",`

`"QTLIB": "/usr/lib64/qt-3.3/lib",`

`"SHELL": "/bin/bash",`

`"SHLVL": "2",`

`"SSH_ASKPASS": "/usr/libexec/openssh/gnome-ssh-askpass",`

`"SSH_CLIENT": "172.16.4.100 60862 22",`

`"SSH_CONNECTION": "172.16.4.100 60862 172.16.4.101 22",`

`"SSH_TTY": "/dev/pts/0",`

`"TERM": "linux",`

`"USER": "root",`

`"_": "/usr/bin/python"`

`},`

`"ansible_eth0": {`

`"active": true,`

`"device": "eth0",`

`"ipv4": {`

`"address": "172.16.4.101",`

`"netmask": "255.255.0.0",`

`"network": "172.16.0.0"`

`},`

`"ipv6": [`

`{`

`"address": "fe80::20c:29ff:fef1:ddb2",`

`"prefix": "64",`

`"scope": "link"`

`}`

`],`

`"macaddress": "00:0c:29:f1:dd:b2",`

`"module": "e1000",`

`"mtu": 1500,`

`"promisc": false,`

`"type": "ether"`

`},`

`"ansible_form_factor": "Other",`

`"ansible_fqdn": "node2",`

`"ansible_hostname": "node2",`

`"ansible_interfaces": [`

`"lo",`

`"eth0"`

`],`

`"ansible_kernel": "2.6.32-504.el6.x86_64",`

`"ansible_lo": {`

`"active": true,`

`"device": "lo",`

`"ipv4": {`

`"address": "127.0.0.1",`

`"netmask": "255.0.0.0",`

`"network": "127.0.0.0"`

`},`

`"ipv6": [`

`{`

`"address": "::1",`

`"prefix": "128",`

`"scope": "host"`

`}`

`],`

`"mtu": 65536,`

`"promisc": false,`

`"type": "loopback"`

`},`

`"ansible_lsb": {`

`"codename": "Final",`

`"description": "CentOS release 6.6 (Final)",`

`"id": "CentOS",`

`"major_release": "6",`

`"release": "6.6"`

`},`

`"ansible_machine": "x86_64",`

`"ansible_memfree_mb": 68,`

`"ansible_memtotal_mb": 474,`

`"ansible_mounts": [`

`{`

`"device": "/dev/mapper/vg0-root",`

`"fstype": "ext4",`

`"mount": "/",`

`"options": "rw",`

`"size_available": 19614814208,`

`"size_total": 21003628544`

`},`

`{`

`"device": "/dev/sda1",`

`"fstype": "ext4",`

`"mount": "/boot",`

`"options": "rw",`

`"size_available": 159812608,`

`"size_total": 198902784`

`},`

`{`

`"device": "/dev/mapper/vg0-usr",`

`"fstype": "ext4",`

`"mount": "/usr",`

`"options": "rw",`

`"size_available": 7437721600,`

`"size_total": 10434699264`

`},`

`{`

`"device": "/dev/mapper/vg0-var",`

`"fstype": "ext4",`

`"mount": "/var",`

`"options": "rw",`

`"size_available": 19657154560,`

`"size_total": 21003628544`

`}`

`],`

`"ansible_nodename": "node2",`

`"ansible_os_family": "RedHat",`

`"ansible_pkg_mgr": "yum",`

`"ansible_processor": [`

`"Intel(R) Core(TM) i5-4200M CPU @ 2.50GHz"`

`],`

`"ansible_processor_cores": 1,`

`"ansible_processor_count": 1,`

`"ansible_processor_threads_per_core": 1,`

`"ansible_processor_vcpus": 1,`

`"ansible_product_name": "VMware Virtual Platform",`

`"ansible_product_serial": "VMware-56 4d cf 50 b9 67 e1 67-fd ba 73 89 3c f1 dd b2",`

`"ansible_product_uuid": "564DCF50-B967-E167-FDBA-73893CF1DDB2",`

`"ansible_product_version": "None",`

`"ansible_python_version": "2.6.6",`

`"ansible_selinux": {`

`"status": "disabled"`

`},`

`"ansible_ssh_host_key_dsa_public": "AAAAB3NzaC1kc3MAAACBAJX6mCvtR/7SlWAQtqoJ7fGcOnCQUif9z8BNlTf8Zh0WadsNAOOm6bq48O8FKAuuUl+PDQdvfJteGSb0maqdzBL1A5BaOCIBFLAdcubrSPm9BjBu37M9Dd0/rGzeLmNRpKuEf0VpmJ0QJvp36iDnDsUCtekjrYlhhwcauJUSVqvxAAAAFQDVMMCA/SHSLd8bskYGRZztoVDHewAAAIB16IbxqPLWALteNs8HmFkk2+m43LQ8xJhzAoUB/rBYclWs0W8HHKF98p91rYfwVfitLIZ+S2gFEYxrMrMyRlJNpeChDDeEVZoMyhBeo8Y9PXd4eQCl+FJ2ZQJ31lC0EEB6T+zwdKuP5dQfCaJ7wVNr87UQuQf7t8My3GHkN7ErSAAAAIEAhPngSDosZGPofwHzNJG+dDITWQEJ7cI0tpYJUum0xREDnkYuWUhjI5soR4jotfToUeTwLRxRZMJXA7dCE6NtHJicEbhxiSXHYQUNhQvvrE0/MEvH/BChJK0Nqqt/RxpFNMF2yN5lJPYdufSKd92rgYEMIzELuh21GL6c4iGJWyM=",`

`"ansible_ssh_host_key_rsa_public": "AAAAB3NzaC1yc2EAAAABIwAAAQEA57ZIETHEsImga9O6N6pHClC6vXfhK01/odan1OdlZnCGTSx3J/fhvNxRidj6YwTb91lahWRRZHOx6EuQ35O4Ba/YVdOF/eF47i/tFNHuo5eEXj2n6fV1mHS97rBc2P+o+2TwL0Ezo9ifXjQPyUHRcCir0abh/if7m/DNfxofY0trW0MKKlA9Ig3CuKrg1k5DUD2HYTS2bbWXDppDG2hpXa6DgxiFO6iLWysR9rw6kY5Aj8WusUpLd2aDkolfIV4zHn2TF6tLP0c1pF2z3u7F5IjOg7aqbEfubmI7mU2d6VPVbxsAakvBNGXoClwt8Skxki1U4u4KXSZ8s/Vq75nRIQ==",`

`"ansible_swapfree_mb": 2047,`

`"ansible_swaptotal_mb": 2047,`

`"ansible_system": "Linux",`

`"ansible_system_vendor": "VMware, Inc.",`

`"ansible_user_id": "root",`

`"ansible_userspace_architecture": "x86_64",`

`"ansible_userspace_bits": "64",`

`"ansible_virtualization_role": "guest",`

`"ansible_virtualization_type": "VMware",`

`"module_setup": true`

`},`

`"changed": false`

`}`

