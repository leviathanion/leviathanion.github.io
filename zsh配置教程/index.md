# Zsh配置教程

# Zsh配置教程
* 安装zsh
* 安装zsh-autosuggestions(自动建议插件)
* 安装zsh-syntax-highlighting(代码高亮插件)
* 安装zsh-theme-powerlevel10k(主题)
    * 配置完成后，可使用`p10k`命令调整配置，具体可看`p10k --help`
* 更改默认sh`chsh -s /bin/zsh`
* 当不存在.zshrc文件时，会自动启动`zsh-newuser-install`来进行初始化设置
    * 如果未出现或者希望重新配置，可以按如下命令执行
    ```shell
    autoload -Uz zsh-newuser-install
    zsh-newuser-install -f
    ```
    * 在`zsh-newuser-install`中配置的默认补全，等效于在.zshrc中添加如下
    ```
    autoload -Uz compinit
    compinit
    ```
    * 若不想自己配置，可以安装`grml-zsh-config`
* 在~/.zshrc中添加如下代码，启用插件和主题
```
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme
```
## zsh配置conda
* cd anaconda的安装目录下的bin目录
* 执行conda init zsh `conda init zsh`
> 优化conda速度(注释掉费时代码)：
```shell
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
# __conda_setup="$('/opt/miniconda/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
# if [ $? -eq 0 ]; then
#     eval "$__conda_setup"
# else
    if [ -f "/opt/miniconda/etc/profile.d/conda.sh" ]; then
        . "/opt/miniconda/etc/profile.d/conda.sh"
    else
        export PATH="/opt/miniconda/bin:$PATH"
    fi
# fi
# unset __conda_setup
# <<< conda initialize <<<
```

