# **linux sed命令详解**

Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等

sed 是一种在线编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。

**sed使用参数**

复制代码

代码如下:

\[root@www ~\]\# sed \[-nefr\] \[动作\]

选项与参数：

-n ：使用安静\(silent\)模式。在一般 sed 的用法中，所有来自 STDIN 的数据一般都会被列出到终端上。但如果加上 -n 参数后，则只有经过sed 特殊处理的那一行\(或者动作\)才会被列出来。

-e ：直接在命令列模式上进行 sed 的动作编辑；

-f ：直接将 sed 的动作写在一个文件内， -f filename 则可以运行 filename 内的 sed 动作；

-r ：sed 的动作支持的是延伸型正规表示法的语法。\(默认是基础正规表示法语法\)

-i ：直接修改读取的文件内容，而不是输出到终端。&lt;\/p&gt; &lt;p&gt;动作说明： \[n1\[,n2\]\]function

n1, n2 ：不见得会存在，一般代表『选择进行动作的行数』，举例来说，如果我的动作是需要在 10 到 20 行之间进行的，则『 10,20\[动作行为\] 』&lt;\/p&gt; &lt;p&gt;function：

a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现\(目前的下一行\)～

c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！

d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；

i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现\(目前的上一行\)；

p ：列印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～

s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s\/old\/new\/g 就是啦！

以行为单位的新增\/删除

将 \/etc\/passwd 的内容列出并且列印行号，同时，请将第 2~5 行删除！

复制代码

代码如下:

\[root@www ~\]\# nl \/etc\/passwd \| sed '2,5d'

1 root:x:0:0:root:\/root:\/bin\/bash

6 sync:x:5:0:sync:\/sbin:\/bin\/sync

7 shutdown:x:6:0:shutdown:\/sbin:\/sbin\/shutdown

.....\(后面省略\).....

sed 的动作为 '2,5d' ，那个 d 就是删除！因为 2-5 行给他删除了，所以显示的数据就没有 2-5 行罗～ 另外，注意一下，原本应该是要下达 sed -e 才对，没有 -e 也行啦！同时也要注意的是， sed 后面接的动作，请务必以 '' 两个单引号括住喔！

只要删除第 2 行

复制代码

代码如下:

nl \/etc\/passwd \| sed '2d'

要删除第 3 到最后一行

复制代码

代码如下:

nl \/etc\/passwd \| sed '3,$d'

在第二行后\(亦即是加在第三行\)加上『drink tea?』字样！

复制代码

代码如下:

\[root@www ~\]\# nl \/etc\/passwd \| sed '2a drink tea'

1 root:x:0:0:root:\/root:\/bin\/bash

2 bin:x:1:1:bin:\/bin:\/sbin\/nologin

drink tea

3 daemon:x:2:2:daemon:\/sbin:\/sbin\/nologin

.....\(后面省略\).....

那如果是要在第二行前

复制代码

代码如下:

nl \/etc\/passwd \| sed '2i drink tea'

如果是要增加两行以上，在第二行后面加入两行字，例如『Drink tea or .....』与『drink beer?』

复制代码

代码如下:

\[root@www ~\]\# nl \/etc\/passwd \| sed '2a Drink tea or ......\

&gt; drink beer ?'

root:x:0:0:root:\/root:\/bin\/bash

bin:x:1:1:bin:\/bin:\/sbin\/nologin

Drink tea or ......

drink beer ?

daemon:x:2:2:daemon:\/sbin:\/sbin\/nologin

.....\(后面省略\).....

每一行之间都必须要以反斜杠『 \ 』来进行新行的添加喔！所以，上面的例子中，我们可以发现在第一行的最后面就有 \ 存在。

以行为单位的替换与显示

将第2-5行的内容取代成为『No 2-5 number』呢？

复制代码

代码如下:

\[root@www ~\]\# nl \/etc\/passwd \| sed '2,5c No 2-5 number'

1 root:x:0:0:root:\/root:\/bin\/bash

No 2-5 number

6 sync:x:5:0:sync:\/sbin:\/bin\/sync

.....\(后面省略\).....

透过这个方法我们就能够将数据整行取代了！

仅列出 \/etc\/passwd 文件内的第 5-7 行

复制代码

代码如下:

\[root@www ~\]\# nl \/etc\/passwd \| sed -n '5,7p'

5 lp:x:4:7:lp:\/var\/spool\/lpd:\/sbin\/nologin

6 sync:x:5:0:sync:\/sbin:\/bin\/sync

7 shutdown:x:6:0:shutdown:\/sbin:\/sbin\/shutdown

可以透过这个 sed 的以行为单位的显示功能， 就能够将某一个文件内的某些行号选择出来显示。

**数据的搜寻并显示**

搜索 \/etc\/passwd有root关键字的行

复制代码

代码如下:

nl \/etc\/passwd \| sed '\/root\/p'

root:x:0:0:root:\/root:\/bin\/bash

root:x:0:0:root:\/root:\/bin\/bash

daemon:x:1:1:daemon:\/usr\/sbin:\/bin\/sh

bin:x:2:2:bin:\/bin:\/bin\/sh

sys:x:3:3:sys:\/dev:\/bin\/sh

sync:x:4:65534:sync:\/bin:\/bin\/sync

....下面忽略

如果root找到，除了输出所有行，还会输出匹配行。

使用-n的时候将只打印包含模板的行。

复制代码

代码如下:

nl \/etc\/passwd \| sed -n '\/root\/p'

1 root:x:0:0:root:\/root:\/bin\/bash

**数据的搜寻并删除**

删除\/etc\/passwd所有包含root的行，其他行输出

复制代码

代码如下:

nl \/etc\/passwd \| sed '\/root\/d'

2 daemon:x:1:1:daemon:\/usr\/sbin:\/bin\/sh

3 bin:x:2:2:bin:\/bin:\/bin\/sh

....下面忽略

\#第一行的匹配root已经删除了

**数据的搜寻并执行命令**

找到匹配模式eastern的行后，

搜索\/etc\/passwd,找到root对应的行，执行后面花括号中的一组命令，每个命令之间用分号分隔，这里把bash替换为blueshell，再输出这行：

复制代码

代码如下:

nl \/etc\/passwd \| sed -n '\/root\/{s\/bash\/blueshell\/;p}' 1 root:x:0:0:root:\/root:\/bin\/blueshell

如果只替换\/etc\/passwd的第一个bash关键字为blueshell，就退出

复制代码

代码如下:

nl \/etc\/passwd \| sed -n '\/bash\/{s\/bash\/blueshell\/;p;q}'

1 root:x:0:0:root:\/root:\/bin\/blueshell

最后的q是退出。

**数据的搜寻并替换**

除了整行的处理模式之外， sed 还可以用行为单位进行部分数据的搜寻并取代。基本上 sed 的搜寻与替代的与 vi 相当的类似！他有点像这样：

复制代码

代码如下:

sed 's\/要被取代的字串\/新的字串\/g'

先观察原始信息，利用 \/sbin\/ifconfig 查询 IP

复制代码

代码如下:

\[root@www ~\]\# \/sbin\/ifconfig eth0

eth0 Link encap:Ethernet HWaddr 00:90:CC:A6:34:84

inet addr:192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0

inet6 addr: fe80::290:ccff:fea6:3484\/64 Scope:Link

UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1

.....\(以下省略\).....

本机的ip是192.168.1.100。

将 IP 前面的部分予以删除

复制代码

代码如下:

\[root@www ~\]\# \/sbin\/ifconfig eth0 \| grep 'inet addr' \| sed 's\/^.\*addr:\/\/g'

192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0

接下来则是删除后续的部分，亦即： 192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0

将 IP 后面的部分予以删除

复制代码

代码如下:

\[root@www ~\]\# \/sbin\/ifconfig eth0 \| grep 'inet addr' \| sed 's\/^.\*addr:\/\/g' \| sed 's\/Bcast.\*$\/\/g'

192.168.1.100

多点编辑

一条sed命令，删除\/etc\/passwd第三行到末尾的数据，并把bash替换为blueshell

复制代码

代码如下:

nl \/etc\/passwd \| sed -e '3,$d' -e 's\/bash\/blueshell\/'

1 root:x:0:0:root:\/root:\/bin\/blueshell

2 daemon:x:1:1:daemon:\/usr\/sbin:\/bin\/sh

-e表示多点编辑，第一个编辑命令删除\/etc\/passwd第三行到末尾的数据，第二条命令搜索bash替换为blueshell。

直接修改文件内容\(危险动作\)

sed 可以直接修改文件的内容，不必使用管道命令或数据流重导向！ 不过，由於这个动作会直接修改到原始的文件，所以请你千万不要随便拿系统配置来测试！ 我们还是使用下载的 regular\_express.txt 文件来测试看看吧！

利用 sed 将 regular\_express.txt 内每一行结尾若为 . 则换成 !

复制代码

代码如下:

\[root@www ~\]\# sed -i 's\/.$\/!\/g' regular\_express.txt

利用 sed 直接在 regular\_express.txt 最后一行加入『\# This is a test』

复制代码

代码如下:

\[root@www ~\]\# sed -i '$a \# This is a test' regular\_express.txt

由於 $ 代表的是最后一行，而 a 的动作是新增，因此该文件最后新增『\# This is a test』！

sed 的『 -i 』选项可以直接修改文件内容，这功能非常有帮助！举例来说，如果你有一个 100 万行的文件，你要在第 100 行加某些文字，此时使用 vim 可能会疯掉！因为文件太大了！那怎办？就利用 sed 啊！透过 sed 直接修改\/取代的功能，你甚至不需要使用 vim 去修订！

流编辑器sed:

sed一次处理一行文件并把输出送往屏幕。sed把当前处理的行存储在临时缓冲区中，称为模式空间\(pattern space\)。一旦sed完成对模式空间中的行的处理，模式空间中的行就被送往屏幕。行被处理完成之后，就被移出模式空间，程序接着读入下一行，处理，显示，移出......文件输入的最后一行被处理完以后sed结束。通过存储每一行在临时缓冲区，然后在缓冲区中操作该行，保证了原始文件不会被破坏。

1. sed的命令和选项：

| **命令** | **功能描述** |
| --- | --- |
| a\ | 在当前行的后面加入一行或者文本。 |
| c\ | 用新的文本改变或者替代本行的文本。 |
| d | 从pattern space位置删除行。 |
| i\ | 在当前行的上面插入文本。 |
| h | 拷贝pattern space的内容到holding buffer\(特殊缓冲区\)。 |
| H | 追加pattern space的内容到holding buffer。 |
| g | 获得holding buffer中的内容，并替代当前pattern space中的文本。 |
| G | 获得holding buffer中的内容，并追加到当前pattern space的后面。 |
| n | 读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。 |
| p | 打印pattern space中的行。 |
| P | 打印pattern space中的第一行。 |
| q | 退出sed。 |
| w file | 写并追加pattern space到file的末尾。 |
| ! | 表示后面的命令对所有没有被选定的行发生作用。 |
| s\/re\/string | 用string替换正则表达式re。 |
| = | 打印当前行号码。 |
| **替换标记** |  |
| g | 行内全面替换，如果没有g，只替换第一个匹配。 |
| p | 打印行。 |
| x | 互换pattern space和holding buffer中的文本。 |
| y | 把一个字符翻译为另一个字符\(但是不能用于正则表达式\)。 |
| **选项** |  |
| -e | 允许多点编辑。 |
| -n | 取消默认输出。 |

_需要说明的是，sed中的正则和grep的基本相同，完全可以参照本系列的第一篇中的详细说明。_

**2. sed实例：**

_ \/&gt; cat testfile_

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

_ \/&gt; sed '\/north\/p' testfile_ \#如果模板north被找到，sed除了打印所有行之外，还有打印匹配行。

northwest NW Charles Main 3.0 .98 3 34

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

\#-n选项取消了sed的默认行为。在没有-n的时候，包含模板的行被打印两次，但是在使用-n的时候将只打印包含模板的行。

_ \/&gt; sed -n '\/north\/p' testfile_

northwest NW Charles Main 3.0 .98 3 34

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

_ \/&gt; sed '3d' testfile_ \#第三行被删除，其他行默认输出到屏幕。

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

_ \/&gt; sed '3,$d' testfile_ \#从第三行删除到最后一行，其他行被打印。$表示最后一行。

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

_ \/&gt; sed '$d' testfile_ \#删除最后一行，其他行打印。

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

_ \/&gt; sed '\/north\/d' testfile_ \#删除所有包含north的行，其他行打印。

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

central CT Ann Stephens 5.7 .94 5 13

\#s表示替换，g表示命令作用于整个当前行。如果该行存在多个west，都将被替换为north，如果没有g，则只是替换第一个匹配。

_ \/&gt; sed 's\/west\/north\/g' testfile_

northnorth NW Charles Main 3.0 .98 3 34

northern WE Sharon Gray 5.3 .97 5 23

southnorth SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

_ \/&gt; sed -n 's\/^west\/north\/p' testfile_ \#-n表示只打印匹配行，如果某一行的开头是west，则替换为north。

northern WE Sharon Gray 5.3 .97 5 23

\#&符号表示替换字符串中被找到的部分。所有以两个数字结束的行，最后的数字都将被它们自己替换，同时追加.5。

_ \/&gt; sed 's\/\[0-9\]\[0-9\]$\/&.5\/' testfile_

northwest NW Charles Main 3.0 .98 3 34.5

western WE Sharon Gray 5.3 .97 5 23.5

southwest SW Lewis Dalsass 2.7 .8 2 18.5

southern SO Suan Chin 5.1 .95 4 15.5

southeast SE Patricia Hemenway 4.0 .7 4 17.5

eastern EA TB Savage 4.4 .84 5 20.5

northeast NE AM Main Jr. 5.1 .94 3 13.5

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13.5

_ \/&gt; sed -n 's\/Hemenway\/Jones\/gp' testfile_ \#所有的Hemenway被替换为Jones。-n选项加p命令则表示只打印匹配行。

southeast SE Patricia Jones 4.0 .7 4 17

\#模板Mar被包含在一对括号中，并在特殊的寄存器中保存为tag 1，它将在后面作为\1替换字符串，Margot被替换为Marlianne。

_ \/&gt; sed -n 's\/\\(Mar\\)got\/\1lianne\/p' testfile_

north NO Marlianne Weber 4.5 .89 5 9

\#s后面的字符一定是分隔搜索字符串和替换字符串的分隔符，默认为斜杠，但是在s命令使用的情况下可以改变。不论什么字符紧跟着s命令都认为是新的分隔符。这个技术在搜索含斜杠的模板时非常有用，例如搜索时间和路径的时候。

_ \/&gt; sed 's\#3\#88\#g' testfile_

northwest NW Charles Main 88.0 .98 88 884

western WE Sharon Gray 5.88 .97 5 288

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 88 188

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 188

\#所有在模板west和east所确定的范围内的行都被打印，如果west出现在esst后面的行中，从west开始到下一个east，无论这个east出现在哪里，二者之间的行都被打印，即使从west开始到文件的末尾还没有出现east，那么从west到末尾的所有行都将打印。

_ \/&gt; sed -n '\/west\/,\/east\/p' testfile_

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

_ \/&gt; sed -n '5,\/^northeast\/p' testfile_ \#打印从第五行开始到第一个以northeast开头的行之间的所有行。

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

\#-e选项表示多点编辑。第一个编辑命令是删除第一到第三行。第二个编辑命令是用Jones替换Hemenway。

_ \/&gt; sed -e '1,3d' -e 's\/Hemenway\/Jones\/' testfile_

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Jones 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

_ \/&gt; sed -n '\/north\/w newfile' testfile_ \#将所有匹配含有north的行写入newfile中。

\/&gt; cat newfile

northwest NW Charles Main 3.0 .98 3 34

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

_ \/&gt; sed '\/eastern\/i\ NEW ENGLAND REGION' testfile_ \#i是插入命令，在匹配模式行前插入文本。

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

NEW ENGLAND REGION

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

\#找到匹配模式eastern的行后，执行后面花括号中的一组命令，每个命令之间用逗号分隔，n表示定位到匹配行的下一行，s\/AM\/Archie\/完成Archie到AM的替换，p和-n选项的合用，则只是打印作用到的行。

_ \/&gt; sed -n '\/eastern\/{n;s\/AM\/Archie\/;p}' testfile_

northeast NE Archie Main Jr. 5.1 .94 3 13

\#-e表示多点编辑，第一个编辑命令y将前三行中的所有小写字母替换为大写字母，-n表示不显示替换后的输出，第二个编辑命令将只是打印输出转换后的前三行。注意y不能用于正则。

_ \/&gt; sed -n -e '1,3y\/abcdefghijklmnopqrstuvwxyz\/ABCDEFGHIJKLMNOPQRSTUVWXYZ\/' -e '1,3p' testfile_

NORTHWEST NW CHARLES MAIN 3.0 .98 3 34

WESTERN WE SHARON GRAY 5.3 .97 5 23

SOUTHWEST SW LEWIS DALSASS 2.7 .8 2 18

_ \/&gt; sed '2q' testfile_ \#打印完第二行后退出。

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

\#当模板Lewis在某一行被匹配，替换命令首先将Lewis替换为Joseph，然后再用q退出sed。

_ \/&gt; sed '\/Lewis\/{s\/Lewis\/Joseph\/;q;}' testfile_

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Joseph Dalsass 2.7 .8 2 18

\#在sed处理文件的时候，每一行都被保存在pattern space的临时缓冲区中。除非行被删除或者输出被取消，否则所有被处理过的行都将打印在屏幕上。接着pattern space被清空，并存入新的一行等待处理。在下面的例子中，包含模板的northeast行被找到，并被放入pattern space中，h命令将其复制并存入一个称为holding buffer的特殊缓冲区内。在第二个sed编辑命令中，当达到最后一行后，G命令告诉sed从holding buffer中取得该行，然后把它放回到pattern space中，且追加到现在已经存在于模式空间的行的末尾。

_ \/&gt; sed -e '\/northeast\/h' -e '$G' testfile_

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

northeast NE AM Main Jr. 5.1 .94 3 13

\#如果模板WE在某一行被匹配，h命令将使得该行从pattern space中复制到holding buffer中，d命令在将该行删除，因此WE匹配行没有在原来的位置被输出。第二个命令搜索CT，一旦被找到，G命令将从holding buffer中取回行，并追加到当前pattern space的行末尾。简单的说，WE所在的行被移动并追加到包含CT行的后面。

_ \/&gt; sed -e '\/WE\/{h;d;}' -e '\/CT\/{G;}' testfile_

northwest NW Charles Main 3.0 .98 3 34

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

central CT Ann Stephens 5.7 .94 5 13

western WE Sharon Gray 5.3 .97 5 23

\#第一个命令将匹配northeast的行从pattern space复制到holding buffer，第二个命令在读取的文件的末尾时，g命令告诉sed从holding buffer中取得行，并把它放回到pattern space中，以替换已经存在于pattern space中的。简单说就是包含模板northeast的行被复制并覆盖了文件的末尾行。

_ \/&gt; sed -e '\/northeast\/h' -e '$g' testfile_

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

northeast NE AM Main Jr. 5.1 .94 3 13

\#模板WE匹配的行被h命令复制到holding buffer，再被d命令删除。结果可以看出WE的原有位置没有输出。第二个编辑命令将找到匹配CT的行，g命令将取得holding buffer中的行，并覆盖当前pattern space中的行，即匹配CT的行。简单的说，任何包含模板northeast的行都将被复制，并覆盖包含CT的行。

_ \/&gt; sed -e '\/WE\/{h;d;}' -e '\/CT\/{g;}' testfile_

northwest NW Charles Main 3.0 .98 3 34

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

north NO Margot Weber 4.5 .89 5 9

western WE Sharon Gray 5.3 .97 5 23

\#第一个编辑中的h命令将匹配Patricia的行复制到holding buffer中，第二个编辑中的x命令，会将holding buffer中的文本考虑到pattern space中，而pattern space中的文本被复制到holding buffer中。因此在打印匹配Margot行的地方打印了holding buffer中的文本，即第一个命令中匹配Patricia的行文本，第三个编辑命令会将交互后的holding buffer中的文本在最后一行的后面打印出来。

_ \/&gt; sed -e '\/Patricia\/h' -e '\/Margot\/x' -e '$G' testfile_

northwest NW Charles Main 3.0 .98 3 34

western WE Sharon Gray 5.3 .97 5 23

southwest SW Lewis Dalsass 2.7 .8 2 18

southern SO Suan Chin 5.1 .95 4 15

southeast SE Patricia Hemenway 4.0 .7 4 17

eastern EA TB Savage 4.4 .84 5 20

northeast NE AM Main Jr. 5.1 .94 3 13

southeast SE Patricia Hemenway 4.0 .7 4 17

central CT Ann Stephens 5.7 .94 5 13



参考:

1. **linux sed命令详解:** http:\/\/www.jb51.net\/LINUXjishu\/144593.html


