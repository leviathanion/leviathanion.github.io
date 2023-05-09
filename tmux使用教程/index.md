# Tmux使用

# Tmux使用教程
## 会话管理
* 新建会话`tmux new -s <session-name>`
* 列出所有会话`tmux ls`或者`Ctrl+b s`
* 退出会话`tmux detach`或者`Ctrl+b d`
* 接入会话`tmux attach -t <编号>/<session-name>`
* 杀死对话`tmux kill-session -t <编号>/<session-name>`
* 切换会话`tmux switch -t <编号>/<session-name>`
* 重命名会话`tmux rename-session -t <编号>/<session-name>`或者`Ctrl+b $`
## 窗口管理
* 创建一个新的窗口`Ctrl+b c`
* 列出所有窗口`Ctrl+b w`
* 重命名当前窗口`Ctrl+b ,`
## 窗格管理
* 划分左右两个窗格`Ctrl+b %`
* 划分上下两个窗格`Ctrl+b "`
* 移动窗格`Ctrl+b 方向键`
* 关闭当前窗格`Ctrl+b x`
* 调整窗格大小`Ctrl+b Ctrl+方向键`

