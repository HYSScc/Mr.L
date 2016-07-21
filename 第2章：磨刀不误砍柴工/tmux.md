# Tmux
转自: [http://blog.chinaunix.net/uid-26495963-id-3140087.html](http://blog.chinaunix.net/uid-26495963-id-3140087.html)

tmux功能：

```  
  提供了强劲的、易于使用的命令行界面。
  可横向和纵向分割窗口。
  窗格可以自由移动和调整大小，或直接利用四个预设布局之一。
  支持 UTF-8 编码及 256 色终端。
  可在多个缓冲区进行复制和粘贴。
  可通过交互式菜单来选择窗口、会话及客户端。
  支持跨窗口搜索。
  支持自动及手动锁定窗口。
```

安装：
         
> sudo apt-get install tmux

或下载源码包安装

-- 基本使用
```
tmux   # 运行 tmux -2 以256终端运行
C-b d  # 返回主 shell ， tmux 依旧在后台运行，里面的命令也保持运行状态
tmux ls # 显示已有tmux会话（C-b s）
tmux attach-session -t 数字 # 选择tmux
tmux new-session -s session-name
tmux kill-session -t session-name
```

-- 快捷键

tmux 的使用主要就是依靠快捷键，通过 C-b 来调用。
```
C-b ?  // 显示快捷键帮助
C-b C-o  //调换窗口位置
C-b 空格键  //采用下一个内置布局
C-b ! // 把当前窗口变为新窗口
C-b  "  // 模向分隔窗口
C-b % // 纵向分隔窗口
C-b q // 显示分隔窗口的编号
C-b o // 跳到下一个分隔窗口
C-b 上下键 // 上一个及下一个分隔窗口
C-b C-方向键 //调整分隔窗口大小
C-b & // 确认后退出窗口 tmux
C-b c // 创建新窗口
C-b 0~9 //选择几号窗口
C-b c // 创建新窗口
C-b n // 选择下一个窗口
C-b l // 最后使用的窗口
C-b p // 选择前一个窗口
C-b w // 以菜单方式显示及选择窗口
C-b s // 以菜单方式显示和选择会话
C-b t //显示时钟
C-b [ 复制(空格开始)
C-b ] 粘贴（回车结束）
C-b ,　给当前窗口改名
```
配置：
配置文件位于：~/.tmux.conf

```
# author: XuTao
# time:	Sun Jul 15 21:57:17 CST 2012
# Usage: mv tmux_conf.txt ~/.tmux.conf
#------------------------------------------

#-- base --#
set -g default-terminal "screen"
set -g display-time 3000
set -g history-limit 65535
#----------------------------------------------

#将默认按键前缀改为与C-i避免与终端快捷键冲突

set-option -g prefix C-i
unbind-key C-b
bind-key C-i send-prefix
#----------------------------------------------

#按键绑定


#水平或垂直分割窗口 (C+A+ :split-window + v/h)
unbind '"'
bind - splitw -v #分割成上下两个窗口
unbind %
bind | splitw -h #分割成左右两个窗口
#----------------------------------------------

#选择分割的窗格
bind k selectp -U #选择上窗格
bind j selectp -D #选择下窗格
bind h selectp -L #选择左窗格
bind l selectp -R #选择右窗格
#----------------------------------------------

#重新调整窗格的大小
bind ^k resizep -U 10
bind ^j resizep -D 10
bind ^h resizep -L 10
bind ^l resizep -R 10
#----------------------------------------------

#交换两个窗格
bind ^u swapp -U
bind ^d swapp -D

bind ^a last
bind q killp
#----------------------------------------------

bind '~' splitw htop
bind ! splitw ncmpcpp
bind m command-prompt "splitw -h 'exec man %%'"

unbind s
#----------------------------------------------

#定制状态行

set -g status-left "#[fg=white,bg=blue] > #I < #[default] |" # 0:bash
#set -g status-left "#[fg=white,bg=blue] > #I < #[default] |" # session-name
set -g status-right "#[fg=yellow,bright][ #[fg=cyan]#W #[fg=yellow]]#[default] #[fg=yellow,bright]- %Y.%m.%d #[fg=green]%H:%M #[default]"
set -g status-right-attr bright

set -g status-bg black
set -g status-fg white
set -g set-clipboard on

setw -g window-status-current-attr bright
#setw -g window-status-current-bg red
setw -g window-status-current-bg green
setw -g window-status-current-fg white

set -g status-utf8 on
set -g status-interval 1

#set -g visual-activity on
#setw -g monitor-activity on

set -g status-keys vi
#----------------------------------------------

setw -g mode-keys vi
setw -g mode-mouse on

#setw -g mouse-resize-pane on
#setw -g mouse-select-pane on
#setw -g mouse-select-window on

# move x clipboard into tmux paste buffer
bind C-p run "tmux set-buffer \"$(xclip -o -sel clipbaord)\"; tmux paste-buffer"
# move tmux copy buffer into x clipboard
bind C-y run "tmux show-buffer | xclip -i -sel clipbaord"

#默认启动应用

#new -s work # 新建名为 work 的会话，并启动 mutt
#neww rtorrent # 启动 rtorrent
#neww vim # 启动 vim
#neww zsh
#selectw -t 3 # 默认选择标号为 3 的窗口
```

更多功能请 man tmux 或进入 tmux 后 C-b ? 
