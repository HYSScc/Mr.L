转自:org-mode入门教程: http:\/\/www.fuzihao.org\/blog\/2015\/02\/19\/org-mode%E6%95%99%E7%A8%8B\/

# **org-mode 入门教程**

org-mode是Emacs提供的一个强大的编辑模式，可以用于做会议笔记以及制作各种待办事项（GDT）。其语法类似于Markdown但是提供了比Markdown更多的操作，再加上Emacs强大的编辑功能，能给笔记增加很多动态的操作（能纯文本上实现折叠、展开、树状视图、表格求和、求代码运行结果等功能），可以说org-mode是最强大的标记语言。而org-mode的强大，也导致了他比markdown更加复杂，需要花些时间来练习，本文选出了org-mode最强大、实用的功能，进行最简单的介绍，下面介绍org-mode的使用方法。

## **安装**

[Emacs](http://www.gnu.org/software/emacs/)最新版本（24.4）自带org-mode，这就意味着只要打开一个后缀名为org的文件就会自动进入org-mode。因此我们只需要下载最新版的Emacs即可。

windows用户：到[http:\/\/ftp.gnu.org\/gnu\/emacs\/windows\/](http://ftp.gnu.org/gnu/emacs/windows/) 下载最新版

ubuntu系列：在终端中输入：`sudo apt-get install emacs`

## **配置**

org-mode功能基本比较完善，不需要更多的配置，但是org模式下默认没有自动换行的功能，我们在.emacs文件里面添加如下代码，实现自动换行：

```
(add-hook 'org-mode-hook (lambda () (setq truncate-lines nil)))

```

## **实例教程**

以下分别讲解org-mode下几个实用的功能，更完整的教程可以参考[官网教程](http://orgmode.org/orgguide.pdf)。下面一步一步完成我们的org文件。（本教程中我们定义 `C-x` 表示按Ctrl+x，`M+x`标识Alt+x，`S+x`代表Shift+x，这也是Emacs的标准定义）

### **新建文件**

1\) 打开Emacs

2\) 输入 `C-x C-f ～/test/test.org`

注意文件后缀名为org，该命令新建了一个名为test.org的文件。

新建完文件后，我们就可以输入相应的内容了。以下是org-mode语法的具体介绍。

### **章节**

org-mode用`*` 标识章节，一个`*` 代表一级标题，两个`*` 代表两级标题，以此类推。

在Emacs中输入以下内容：

```
* 标题一
** 标题二

```

注意型号后面有空格。可以发现，不同层级标题的颜色是不一样的。按Alt加左右键能够升降标题的层级。一些常用快捷键如下：

* `S+Tab` 展开、折叠所有章节
* `Tab` 对光标所在章节进行展开、折叠
* `M+left/right` 升级\/降级标题

### **列表**

列表是文本中一个比较常用的元素，可以列出各种待完成的事项等。org-mode提供了一种很独特的功能，可以加入checkbok（实际就是加入一对中间有空格的方括号`[ ]`）标记任务的完成状况，而且如果一个总任务有多个子任务，还可以根据子任务的完成情况计算总进度（只需要在总任务后面添加一对方括号，里面加上`%`或`/` 如`[%],[/]`）。列表分为有序和无序两种，有序列表以`1.`或 `1)`开头，无序列表以`+`或`-`开头 后面，同样，后面要跟一个空格。

我们输入一个无序列表：

```
+ treeroot  
    + branch1
    + branch2

```

在输入的时候，我们按`M-RET`（Alt+回车）可以自动输入同级的条目，按`M+left/right`调整层级。同理，我们还可以输入一个有序列表：

```
1) [-] 任务1 [33%]
   1) [ ] 子任务1
   2) [X] 子任务2
   3) [ ] 子任务3
2) [ ] 任务2

```

我们按`M-S-RET` 可以输入一个带checkbox的列表项，而在总任务后面可以输入一个`[%]`或者`[/]`则能自动计算总任务进度。最后结果如图所示。

常用快捷键：

* `M-RET` 插入同级列表项
* `M-S-RET` 插入有 checkbox的同级列表项
* `C-c C-c` 改变 checkbox状态
* `M-left/right` 改变列表项层级关系
* `M-up/dowm` 上下移动列表项

### **脚注**

用`[fn:1]`的方式插入脚注，而在最下面插入

```
[fn:1]本文参考自http://orgmode.org/orgguide.pdf

```

这个标签是可以点击的。

### **表格**

表格常用于数据展示，org-mode提供了方便的列表操作。其中最独特的是支持类似于Excel的表格函数，可以完成简单的求和等操作

创建表格时，在新建表格时，首先输入表头：

```
input | Name  |  Phone | sub1 | sub2 | total |
|-

```

然后在第二行后面按 tab，表格就会自动生成：

```
| Name    |  Phone | sub1 | sub2 | total |
|---------+--------+------+------+-------|
|         |        |      |      |       |

```

下面我们填入一些数据，在填写的过程中，按Tab键可以调到右方表格，按Enter则能跳到下方表格。如果同时按住Shift，则是反方向跳。输入表格完成后，按 `C-c C-c`则能自动对齐表格，出入完成对齐后，表格如下：

```
| Name        |  Phone | sub1 | sub2 | total |
|-------------+--------+------+------+-------|
| maple       | 134... |   89 |   98 |       |
| wizard      | 152... |   78 |   65 |       |
| Hello World | 123... |   76 |   87 |       |
| hehe        | 157... |   87 |   78 |       |

```

下面我们来体验下org-mode的强大的表格函数。我们在total列任选一个位置，输入`=$3+$4` ，然后按`C-u C-c C-c` ，org-mode便能自动为我们计算所有三列加四列的和，并放到第五列。最后完成的表格如下

```
| Name        |  Phone | sub1 | sub2 | total |
|-------------+--------+------+------+-------|
| maple       | 134... |   89 |   98 |   187 |
| wizard      | 152... |   78 |   65 |   143 |
| Hello World | 123... |   76 |   87 |   163 |
| hehe        | 157... |   87 |   78 |   165 | 
# +TBLFM: $5=$3+$4

```

如果要插入行和列，可在表头添加一个标签或者新起一行，输入\|再调整格式即可。其中最后一行是ogr-mode自动添加的。一些快捷键如下：

* `C-c |` 通过输入大小的方式插入表格
* `C-c C-c` 对齐表格
* `Tab` 调到右边一个表格
* `enter` 跳到下方的表格
* `M-up/right/left/right` 上下左右移动行（列）
* `M-S-up/right/left/right` 向上下左右插入行（列）

### **链接**

链接用于链接一些资源地址，如图片、文件、URL等。

链接的格式是：

```
[[链接地址][链接内容]]

```

如：

```
[[http://orgmode.org/orgguide.pdf][grgguid.pdf]]] 
[[file:/home/maple/图片/test.jpg][a picture]]

```

如果去掉标签，则能直接显示图片：

```
[[file:/home/maple/图片/test.jpg]]

```

直接显示的图片在Emacs里默认不显示，需按`C-c C-x C-v`才能显示，在输出成其他格式（html、pdf……）后也能看到。

常用快捷键：

* `C-c C-x C-v` 直接预览图片。

### **待办事项（TODO ）**

TODO是org-mode最具特色的一个功能，也是org-mode设计的初衷，org-mode的作者本意是用其来完成一个个人时间管理程序（GDT）。因此，可以用org-mode来做一个个人时间管理工具！

下面我们来看怎么写TODO。TODO 也是一类标题，因此也需要用`*`开头，在Emacs中输入：

```
** TODO 洗衣服

```

可以看到当中的TODO变成了红色，我们讲光标移到该行，按`C-c C-t`，则发现TODO变成了DONE（这个序列可以自己定义，详见org-mode手册）。org-mode兼有了标题和列表的功能，也可以添加checkbox和完成进度，除此之外，还可以设计优先级。

我们输入：

```
*** TODO [# A] 任务1
*** TODO [# B] 任务2
*** TODO 总任务 [33%]
**** TODO 子任务1
**** TODO 子任务2 [0%]
      - [-] subsub1 [1/2]
       - [ ] subsub2
       - [X] subsub3
    **** DONE 一个已完成的任务

```

一些常用操作如下：

* `C-c C-t` 变换TODO的状态
* `C-c / t`以树的形式展示所有的 TODO
* `C-c C-c` 改变 checkbox状态
* `C-c`, 设置优先级（方括号里的ABC）
* `M-S-RET` 插入同级TODO标签

## **标签Tags**

在org-mode中，可以给每一章节添加一个标签，我们可以通过树的结构来查看所有带标签的章节。在每一节中，子标题的标签会继承父标题标签。

输入：

```
*** 章标题      :work:learn:
**** 节标题1      :fly:plane:
**** 节标题2      :car:run:

```

一些常用命令如下：

* `C-c C-q` 为标题添加标签
* `C-c / m` 生成带标签的树

### **时间**

org-mode可以利用Emacs的时间空间插入当前时间。

输入`C-c .` 会出现一个日历，我们点选相应的时间即可插入。

```
<2015-02-17 二>

```

时间前可以加DEADLINE:和SCHEDULED:表示时间的类型如：

```
DEADLINE:<2015-02-12 四>

```

下面给出一个常见的TODO标签的模板：

```
*** TODO     洗衣服
SCHEDULED: <2015-02-19 四>
DEADLINE: <2015-03-01 日>

```

常用命令如下：

* `C-c .`插入时间

### **一些特殊文本格式**

和其他标记语言一样，org-mode也支持各种特殊文本格式（如斜体、下划线等）。如下所示：

```
*bold*
/italic/
_underlined_
=code=
~verbatim~
+strike-through+

```

### **富文本导出**

org-mode的强大之处还在于它能到出成各种不同的格式，例如html、pdf等，在导出时，可以加入一些说明符号，来制定导出选项，常用导出符号如下：

设置标题和目录：

```
# +TITLE: This is the title of the document
# +OPTIONS: toc:2 (only to two levels in TOC)
# +OPTIONS: toc:nil (no TOC at all)

```

添加引用：

```
# +BEGIN_QUOTE
Everything should be made as simple as possible,
but not any simpler -- Albert Einstein
# +END_QUOTE

```

设置居中：

```
# +BEGIN_CENTER
    Everything should be made as simple as possible,but not any simpler
# +END_CENTER

```

设置样例（在这里面的内容将会被直接输出，不会被转义）

```
# +BEGIN_EXAMPLE
这里面的字符不会被转义
# +END_EXAMPLE

```

注释，这些内容不会被导出

```
注释的用法#  this is comment

# +BEGIN_COMMENT
这里的注释不会被导出
# +END_COMMENT

```

Latex使用，org-mode能支持直接输入LaTeX，在导出后LaTeX能被正确解释：

\begin{equation}

\nabla^2 x=\int_\Omega \frac{a}{\log{b}_

_} \sum^n_{i=1} a\_i d\Omega

\end{equation}

#### **插入源代码**

org-mode除了可以直接插入源代码之外，可以直接求出运行结果，这也是其强大之处，在使用之前，需要在.emacs配置文件中设置加载的运行语言：

```
(org-babel-do-load-languages
 'org-babel-load-languages
 '(
   (sh . t)
   (python . t)
   (R . t)
   (ruby . t)
   (ditaa . t)
   (dot . t)
   (octave . t)
   (sqlite . t)
   (perl . t)
   (C . t)
   ))

```

设置好之后输入：

```
# +BEGIN_SRC emacs-lisp
(+ 1 2 3 4)
# +END_SRC

```

将光标移到代码块内，按`C-c C-c`，org-mode会自动添加如下一行：

```
# +RESULTS:
: 10

```

这正是该代码的计算结果。下面试一试Python代码：

```
# +BEGIN_SRC python :results output
a = 1+1
print a
# +END_SRC

# +RESULTS:
: 2

```

下面测试一下C语言

```
# +begin_src C++ :includes <stdio.h> 
  int a=1;
  int b=1;
  printf("%d\n", a+b);
# +end_src

# +RESULTS:
: 2

```

常用快捷键：

* `C-c C-c` 对当前代码块求值

#### **关于导出**

org-mode导出pdf需要LaTeX支持，而导出html很方便，但是导出的html美柚任何样式，org-mode为每个模块都添加了css的标签，我们可以将现成的css文件直接加入，便能得到一个好看的输出样式了：

在头部加入

```
# +HTML_HEAD: <link rel="stylesheet" type="text/css" href="style1.css" />

```

该代码加载我们的css。下面我们来将我们的文件导出成html。按`C-c C-e`会出现一个如图的提示框，问我们导出那种格式，我们选`h`导出html。

常用快捷键：

* `C-c C-e` 选择相应的导出格式

## **结论**

org-mode是Emacs非常重要的一个模式，自其诞生之后，受到了各种Emacs粉丝的追捧，甚至在VIM上也衍生出了org-mode的分支，足见它的人气。通过本教程介绍，我们已经见识到了org-mode 的强大功能，而这仅仅只是org-mode功能的一小部分，org-mode更多的功能等待这我们去探索、发现。更多文章请访问[切问录](http://www.fuzihao.org/) [http:\/\/www.fuzihao.org](http://www.fuzihao.org/)

## **附件**

org文件[tutorial.org](http://www.fuzihao.org/blog/attachment/tutorial.org)

导出html文件[tutorial.html](http://www.fuzihao.org/blog/attachment/tutorial.html) \(直接点击可以看到效果，点右键另存为即是编译后的文件\)

## **参考文献**

\[1\]org-mode 官方教程 [http:\/\/orgmode.org\/orgguide.pdf](http://orgmode.org/orgguide.pdf)

