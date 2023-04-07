# Zsh配置

# zsh配置
* 安装zsh
* 安装zsh-autosuggestions(自动建议插件)
* 安装zsh-syntax-highlighting(代码高亮插件)
* 安装zsh-theme-powerlevel10k(主题)
* 更改默认sh`chsh -s /bin/zsh`
* 在~/.zshrc中添加如下代码，启用插件和主题
```
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh

source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme
```
## zsh配置conda
* cd anaconda的安装目录下的bin目录
* 执行conda init zsh `conda init zsh`

