# Fcitx5安装

# Fcitx5安装
* 安装fcitx5
* 安装中文addon：fcitx5-chinese-addons
* 安装qt支持：fcitx5-qt
* 安装gtk支持：fcitx5-gtk
* 安装词库：fcitx5-pinyin-zhwiki，fcitx5-pinyin-zhwiki,fcitx5-pinyin-zhwiki
* 配置fcitx5
    * 安装fcitx5-configtool
    * 打开fcitx5-configtool，在input method中添加pinyin
* 编辑`/etc/environment`
```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus
```



