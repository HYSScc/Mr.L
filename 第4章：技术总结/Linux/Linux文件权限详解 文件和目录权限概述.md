在linux中的每一个文件或目录都包含有访问权限，这些访问权限决定了谁能访问和如何访问这些文件和目录。  
  
**通过设定权限可以从以下三种访问方式限制访问权限：**只允许用户自己访问；允许一个预先指定的用户组中的用户访问；允许系统中的任何用户访问。同时，用户能够控制一个给定的文件或目录的访问程度。一个文件活目录可能有读、写及执行权限。当创建一个文件时，系统会自动地赋予文件所有者读和写的权限，这样可以允许所有者能够显示文件内容和修改文件。文件所有者可以将这些权限改变为任何他想指定的权限。一个文件也许只有读权限，禁止任何修改。文件也可能只有执行权限，允许它想一个程序一样执行。  
  
**三种不同的用户类型能够访问一个目录或者文件：**所有着、用户组或其他用户。所有者就是创建文件的用户，用户是所有用户所创建的文件的所有者，用户可以允许所在的用户组能访问用户的文件。通常，用户都组合成用户组，例如，某一类或某一项目中的所有用户都能够被系统管理员归为一个用户组，一个用户能够授予所在用户组的其他成员的文件访问权限。最后，用户也将自己的文件向系统内的所有用户开放，在这种情况下，系统内的所有用户都能够访问用户的目录或文件。在这种意义上，系统内的其他所有用户就是other用户类。  
每一个用户都有它自身的读、写和执行权限。第一套权限控制访问自己的文件权限，即所有者权限。第二套权限控制用户组访问其中一个用户的文件的权限。第三套权限控制其他所有用户访问一个用户的文件的权限，这三套权限赋予用户不同类型（即所有者、用户组和其他用户）的读、写及执行权限就构成了一个有9种类型的权限组。  
  
我们可以用-l参数的ls命令显示文件的详细信息，其中包括权限。如下所示：  
  
yekai@kebao:/media/sda5/软件压缩/Linux$ ls -lh  
总用量 191M  
-rwxrwx--- 1 root plugdev 18M 2007-02-28 18:05 ActionCube\_v0.92.tar.bz2  
-rwxrwx--- 1 root plugdev 60M 2007-04-30 22:52 nexuiz-223.zip  
-rwxrwx--- 1 root plugdev 7.4M 2007-04-25 02:16 stardict-oxford-gb-2.4.2.tar.bz2  
-rwxrwx--- 1 root plugdev 102M 2007-05-01 18:22 tremulous-1.1.0-installer.x86.run  
-rwxrwx--- 1 root plugdev 4.9M 2007-04-30 14:32 wqy-bitmapfont-0.8.1-7\_all.deb.bin  
  
当执行ls -l 或 ls -al 命令后显示的结果中，最前面的第2～10个字符是用来表示权限。第一个字符一般用来区分文件和目录：  
  
d：表示是一个目录，事实上在ext2fs中，目录是一个特殊的文件。  
－：表示这是一个普通的文件。  
l: 表示这是一个符号链接文件，实际上它指向另一个文件。  
b、c：分别表示区块设备和其他的外围设备，是特殊类型的文件。  
s、p：这些文件关系到系统的数据结构和管道，通常很少见到。  
下面详细介绍一下权限的种类和设置权限的方法。

**一般权限**

第2～10个字符当中的每3个为一组，左边三个字符表示所有者权限，中间3个字符表示与所有者同一组的用户的权限，右边3个字符是其他用户的权限。这三个一组共9个字符，代表的意义如下：

  
r\(Read，读取\)：对文件而言，具有读取文件内容的权限；对目录来说，具有浏览目录的权  
w\(Write,写入\)：对文件而言，具有新增、修改文件内容的权限；对目录来说，具有删除、移动目录内文件的权限。  
x\(eXecute，执行\)：对文件而言，具有执行文件的权限；对目录了来说该用户具有进入目录的权限。  
  
－：表示不具有该项权限。  
下面举例说明：  
－rwx------: 文件所有者对文件具有读取、写入和执行的权限。  
-rwxr―r--: 文件所有者具有读、写与执行的权限，其他用户则具有读取的权限。  
-rw-rw-r-x: 文件所有者与同组用户对文件具有读写的权限，而其他用户仅具有读取和执行的权限。  
drwx--x--x: 目录所有者具有读写与进入目录的权限,其他用户近能进入该目录，却无法读取任何数据。  
Drwx------: 除了目录所有者具有完整的权限之外，其他用户对该目录完全没有任何权限。  
  
每个用户都拥有自己的专属目录，通常集中放置在/home目录下，这些专属目录的默认权限为rwx------:  
  
表示目录所有者本身具有所有权限，其他用户无法进入该目录。执行mkdir命令所创建的目录，其默认权限为rwxr-xr-x,用户可以根据需要修改目录的权限。  
  
此外，默认的权限可用umask命令修改，用法非常简单，只需执行umask 777 命令，便代表屏蔽所有的权限，因而之后建立的文件或目录，其权限都变成000，依次类推。通常root帐号搭配umask命令的数值为022、027和 077，普通用户则是采用002，这样所产生的权限依次为755、750、700、775。有关权限的数字表示法，后面将会详细说明。  
  
用户登录系统时，用户环境就会自动执行rmask命令来决定文件、目录的默认权限。

**特殊权限**

其实文件与目录设置不止这些，还有所谓的特殊权限。由于特殊权限会拥有一些“特权”，因而用户若无特殊需求，不应该启用这些权限，避免安全方面出现严重漏洞，造成黑客入侵，甚至摧毁系统!!!  
  
s或S（SUID,Set UID）：可执行的文件搭配这个权限，便能得到特权，任意存取该文件的所有者能使用的全部系统资源。请注意具备SUID权限的文件，黑客经常利用这种权限，以SUID配上root帐号拥有者，无声无息地在系统中开扇后门，供日后进出使用。  
  
s或S（SGID，Set GID）：设置在文件上面，其效果与SUID相同，只不过将文件所有者换成用户组，该文件就可以任意存取整个用户组所能使用的系统资源。  
  
T或T（Sticky）：/tmp和 /var/tmp目录供所有用户暂时存取文件，亦即每位用户皆拥有完整的权限进入该目录，去浏览、删除和移动文件。  
  
因为SUID、SGID、Sticky占用x的位置来表示，所以在表示上会有大小写之分。加入同时开启执行权限和SUID、SGID、Sticky，则权限表示字符是小写的：  
  
-rwsr-sr-t 1 root root 4096 6月 23 08：17 conf  
  
如果关闭执行权限，则表示字符会变成大写：  
  
-rwSr-Sr-T 1 root root 4096 6月 23 08：17 conf

**使用文件管理器来改变文件或目录的权限**

如果用户要改变一个文件目录的权限，右击要改变权限的文件或者目录，在弹出的快捷菜单中选择“属性”，系统将打开属性对话框  
  
在“属性”对话框中，单击“权限”标签，就会打开“权限”选项卡。  
  
在这里你可以修改文件或者目录的所有者、组群和其他用户的权限，而且可以设置特殊权  
  
对于特殊权限，最好不要设置，不然会带来很严重的安全问题。  
  
当然，在这里你也可以改变文件和目录的所有者和所属组。

**使用chmod**和数字改变文件或目录的访问权限

文件和目录的权限表示，是用rwx这三个字符来代表所有者、用户组和其他用户的权限。有时候，字符似乎过于麻烦，因此还有另外一种方法是以数字来表示权限，而且仅需三个数字。  
  
r: 对应数值4  
w: 对应数值2  
x：对应数值1  
－：对应数值0  
  
数字设定的关键是mode的取值，一开始许多初学者会被搞糊涂，其实很简单，我们将rwx看成二进制数，如果有则有1表示，没有则有0表示，那么rwx r-x r- -则可以表示成为：  
  
111 101 100  
  
再将其每三位转换成为一个十进制数，就是754。  
  
例如，我们想让a.txt这个文件的权限为：  
  
自己 同组用户 其他用户  
可读 是 是 是  
可写 是 是  
可执行  
  
那么，我们先根据上表得到权限串为：rw-rw-r--，那么转换成二进制数就是110 110 100，再每三位转换成为一个十进制数，就得到664，因此我 们执行命令：  
  
\[root@localhost ~\]\# chmod 664 a.txt  
  
按照上面的规则，rwx合起来就是4+2+1＝7，一个rwxrwxrwx权限全开放的文件，数值表示为777；而完全不开放权限的文件“－－－－－－－－－”其数字表示为000。下面举几个例子：  
  
-rwx------:等于数字表示700。  
-rwxr―r--:等于数字表示744。  
-rw-rw-r-x:等于数字表示665。  
drwx―x―x:等于数字表示711。  
drwx------:等于数字表示700。  
  
在文本模式下，可执行chmod命令去改变文件和目录的权限。我们先执行ls -l 看看目录内的情况：  
  
\[root@localhost ~\]\# ls -l  
  
总用量 368  
  
-rw-r--r-- 1 root root 12172 8月 15 23:18 conkyrc.sample  
drwxr-xr-x 2 root root 48 9月 4 16:32 Desktop  
-r--r--r-- 1 root root 331844 10月 22 21:08 libfreetype.so.6  
drwxr-xr-x 2 root root 48 8月 12 22:25 MyMusic  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth0  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth1  
-rwxr-xr-x 1 root root 512 11月 5 08:08 net.lo  
drwxr-xr-x 2 root root 48 9月 6 13:06 vmware  
  
可以看到当然文件conkyrc.sample文件的权限是644,然后把这个文件的权限改成777。执行下面命令  
  
\[root@localhost ~\]\# chmod 777 conkyrc.sample  
  
然后ls -l看一下执行后的结果：  
  
\[root@localhost ~\]\# ls -l  
  
总用量 368  
  
-rwxrwxrwx 1 root root 12172 8月 15 23:18 conkyrc.sample  
drwxr-xr-x 2 root root 48 9月 4 16:32 Desktop  
-r--r--r-- 1 root root 331844 10月 22 21:08 libfreetype.so.6  
drwxr-xr-x 2 root root 48 8月 12 22:25 MyMusic  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth0  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth1  
-rwxr-xr-x 1 root root 512 11月 5 08:08 net.lo  
drwxr-xr-x 2 root root 48 9月 6 13:06 vmware  
  
可以看到conkyrc.sample文件的权限已经修改为rwxrwxrwx  
  
如果要加上特殊权限，就必须使用4位数字才能表示。特殊权限的对应数值为：  
  
s或 S （SUID）：对应数值4。  
s或 S （SGID）：对应数值2。  
t或 T ：对应数值1。  
  
用同样的方法修改文件权限就可以了  
  
例如：  
  
\[root@localhost ~\]\# chmod 7600 conkyrc.sample  
\[root@localhost ~\]\# ls -l  
  
总用量 368  
  
-rwS--S--T 1 root root 12172 8月 15 23:18 conkyrc.sample  
drwxr-xr-x 2 root root 48 9月 4 16:32 Desktop  
-r--r--r-- 1 root root 331844 10月 22 21:08 libfreetype.so.6  
drwxr-xr-x 2 root root 48 8月 12 22:25 MyMusic  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth0  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth1  
-rwxr-xr-x 1 root root 512 11月 5 08:08 net.lo  
drwxr-xr-x 2 root root 48 9月 6 13:06 vmware  
  
加入想一次修改某个目录下所有文件的权限，包括子目录中的文件权限也要修改，要使用参数－R表示启动递归处理。  
  
例如：  
  
\[root@localhost ~\]\# chmod 777 /home/user 注：仅把/home/user目录的权限设置为rwxrwxrw  
\[root@localhost ~\]\# chmod -R 777 /home/user 注：表示将整个/home/user目录与其中的文件和子目录的权限都设置为rwxrwxrwx

**使用命令chown**改变目录或文件的所有权

文件与目录不仅可以改变权限，其所有权及所属用户组也能修改，和设置权限类似，用户可以通过图形界面来设置，或执行chown命令来修改。  
  
我们先执行ls -l看看目录情况：  
  
\[root@localhost ~\]\# ls -l  
  
总用量 368  
  
-rwxrwxrwx 1 root root 12172 8月 15 23:18 conkyrc.sample  
drwxr-xr-x 2 root root 48 9月 4 16:32 Desktop  
-r--r--r-- 1 root root 331844 10月 22 21:08 libfreetype.so.6  
drwxr-xr-x 2 root root 48 8月 12 22:25 MyMusic  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth0  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth1  
-rwxr-xr-x 1 root root 512 11月 5 08:08 net.lo  
drwxr-xr-x 2 root root 48 9月 6 13:06 vmware  
  
可以看到conkyrc.sample文件的所属用户组为root，所有者为root。  
执行下面命令，把conkyrc.sample文件的所有权转移到用户user:  
  
\[root@localhost ~\]\# chown user conkyrc.sample  
\[root@localhost ~\]\# ls -l  
  
总用量 368  
  
-rwxrwxrwx 1 user root 12172 8月 15 23:18 conkyrc.sample  
drwxr-xr-x 2 root root 48 9月 4 16:32 Desktop  
-r--r--r-- 1 root root 331844 10月 22 21:08 libfreetype.so.6  
drwxr-xr-x 2 root root 48 8月 12 22:25 MyMusic  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth0  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth1  
-rwxr-xr-x 1 root root 512 11月 5 08:08 net.lo  
drwxr-xr-x 2 root root 48 9月 6 13:06 vmware  
  
要改变所属组，可使用下面命令：  
  
\[root@localhost ~\]\# chown :users conkyrc.sample  
\[root@localhost ~\]\# ls -l  
  
总用量 368  
  
-rwxrwxrwx 1 user users 12172 8月 15 23:18 conkyrc.sample  
drwxr-xr-x 2 root root 48 9月 4 16:32 Desktop  
-r--r--r-- 1 root root 331844 10月 22 21:08 libfreetype.so.6  
drwxr-xr-x 2 root root 48 8月 12 22:25 MyMusic  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth0  
-rwxr-xr-x 1 root root 9776 11月 5 08:08 net.eth1  
-rwxr-xr-x 1 root root 512 11月 5 08:08 net.lo  
drwxr-xr-x 2 root root 48 9月 6 13:06 vmware  
  
要修改目录的权限，使用－R参数就可以了，方法和前面一样。

