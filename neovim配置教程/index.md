# Neovim配置教程

# nvim配置教程
## linux
* 从github拉取配置文件并放在.config目录下，命名为nvim
```shell
git clone https://github.com/leviathanion/nvim-config.git  ~/.config/nvim
```
* 可通过`:checkhealth`查看各插件情况
* 在.bashrc或者.zshrc中添加以下代码，使得root权限也可用
```shell
alias sudonvim = "sudo -E nvim"
```
## windows
* 与linux类似，将配置文件放到`c:user/appdata/local/nvim`中

