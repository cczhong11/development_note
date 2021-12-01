
# basic concept


在Tmux逻辑中，需要分清楚Server > Session > Window > Pane这个大小和层级顺序是极其重要的，直接关系到工作效率：

- Server：是整个tmux的后台服务。有时候更改配置不生效，就要使用tmux kill-server来重启tmux。
- Session：是tmux的所有会话。我之前就错把这个session当成窗口用，造成了很多不便里。一般只要保存一个session就足够了。
- Window：相当于一个工作区，包含很多分屏，可以针对每种任务分一个Window。如下载一个Window，编程一个window。
- Pane：是在Window里面的小分屏。最常用也最好用

# common command 

```bash

#启动新session：
$ tmux [new -s 会话名 -n 窗口名]

#恢复session：
$ tmux at [-t 会话名]

#列出所有sessions：
$ tmux ls

#关闭session：
$ tmux kill-session -t 会话名

#关闭整个tmux服务器：
$ tmux kill-server

# 列出所有快捷键，及其对应的 Tmux 命令
$ tmux list-keys

# 列出所有 Tmux 命令及其参数
$ tmux list-commands

# 列出当前所有 Tmux 会话的信息
$ tmux info

# 重新加载当前的 Tmux 配置
$ tmux source-file ~/.tmux.conf
```

## system

| 前缀   | 指令   | 描述                                   |
| ------ | ------ | -------------------------------------- |
| Ctrl+b | ?      | 显示快捷键帮助文档                     |
| Ctrl+b | d      | 断开当前会话                           |
| Ctrl+b | D      | 选择要断开的会话                       |
| Ctrl+b | Ctrl+z | 挂起当前会话                           |
| Ctrl+b | r      | 强制重载当前会话                       |
| Ctrl+b | s      | 显示会话列表用于选择并切换             |
| Ctrl+b | :      | 进入命令行模式，此时可直接输入ls等命令 |
| Ctrl+b | [      | 进入复制模式，按q退出                  |
| Ctrl+b | ]      | 粘贴复制模式中复制的文本               |
| Ctrl+b | ~      | 列出提示信息缓存                       |

## window

| 前缀   | 指令 | 描述                                     |
| ------ | ---- | ---------------------------------------- |
| Ctrl+b | c    | 新建窗口                                 |
| Ctrl+b | &    | 关闭当前窗口                             |
| Ctrl+b | 0~9  | 切换到指定窗口                           |
| Ctrl+b | p    | 切换到上一窗口                           |
| Ctrl+b | n    | 切换到下一窗口                           |
| Ctrl+b | w    | 打开窗口列表，用于且切换窗口             |
| Ctrl+b | ,    | 重命名当前窗口                           |
| Ctrl+b | .    | 修改当前窗口编号（适用于窗口重新排序）   |
| Ctrl+b | f    | 快速定位到窗口（输入关键字匹配窗口名称） |

## panel

| 前缀   | 指令        | 描述                                                           |
| ------ | ----------- | -------------------------------------------------------------- |
| Ctrl+b | "           | 当前面板上下一分为二，下侧新建面板                             |
| Ctrl+b | %           | 当前面板左右一分为二，右侧新建面板                             |
| Ctrl+b | x           | 关闭当前面板（关闭前需输入y or n确认）                         |
| Ctrl+b | z           | 最大化当前面板，再重复一次按键后恢复正常（v1.8版本新增）       |
| Ctrl+b | !           | 将当前面板移动到新的窗口打开（原窗口中存在两个及以上面板有效） |
| Ctrl+b | ;           | 切换到最后一次使用的面板                                       |
| Ctrl+b | q           | 显示面板编号，在编号消失前输入对应的数字可切换到相应的面板     |
| Ctrl+b | {           | 向前置换当前面板                                               |
| Ctrl+b | }           | 向后置换当前面板                                               |
| Ctrl+b | Ctrl+o      | 顺时针旋转当前窗口中的所有面板                                 |
| Ctrl+b | 方向键      | 移动光标切换面板                                               |
| Ctrl+b | o           | 选择下一面板                                                   |
| Ctrl+b | 空格键      | 在自带的面板布局中循环切换                                     |
| Ctrl+b | Alt+方向键  | 以5个单元格为单位调整当前面板边缘                              |
| Ctrl+b | Ctrl+方向键 | 以1个单元格为单位调整当前面板边缘（Mac下                       |
| Ctrl+b | t           | 显示时钟                                                       |

# config

- set-option -g mouse on
  - 支持鼠标
- 状态栏

```
set -g base-index 1           # start windows numbering at 1
set -g status-utf8 on # 状态栏支持utf8
set -g status-interval 1 # 状态栏刷新时间
set -g status-justify left # 状态栏列表左对齐
setw -g monitor-activity on # 非当前窗口有内容更新时在状态栏通知
set -g set-titles on          # set terminal title
set -wg window-status-format " #I #W " # 状态栏窗口名称格式
set -wg window-status-current-format " #I:#W#F " # 状态栏当前窗口名称格式(#I：序号，#w：窗口名称，#F：间隔符)
set -wg window-status-separator "" # 状态栏窗口名称之间的间隔
```
- vim 模式

```
# vi模式，v开始选择，y 复制选择内容到剪贴板
# Use vim bindings
setw -g mode-keys vi
```

- 调整窗口大小

```
# ctrl + k/j/h/l 调整pane大小
# resize pane
bind -r ^k resizep -U 10 # upward (prefix Ctrl+k)
bind -r ^j resizep -D 10 # downward (prefix Ctrl+j)
bind -r ^h resizep -L 10 # to the left (prefix Ctrl+h)
bind -r ^l resizep -R 10 # to the right (prefix Ctrl+l)
```

```
bind r source-file ~/.tmux.conf \; display '~/.tmux.conf sourced'
set -g prefix2 C-a                        # GNU-Screen compatible prefix
bind C-a send-prefix -2
```


# good one

- https://github.com/gpakosz/.tmux

# save tmux session

- https://github.com/tmux-plugins/tmux-continuum