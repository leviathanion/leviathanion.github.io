# 常用软件


# 常用软件总结(Windows篇)

## 社交
* QQ
* wechat
* dingding
* 飞书
* telegram

## 办公
* office全家桶
* office tool plus(office下载与激活)
* 小恐龙公文排版助手
* iSlide(PPT插件)
* 坚果云
* 百度云
* 有道云笔记

## 音视频
* PotPlayer
* 完美解码
* ffmpeg(视频处理开源项目)
* foobar2000(播放器)
* mp3tag/musictag(歌曲tag编辑器)

## 文件类
* Everything(文件搜索)
* WizTree64(磁盘文件占用)
* SpaceSniffer(磁盘文件占用)
* GoodSync(文件同步软件)
* pandoc(文件类型转换)

## 下载
* IDM(多线程下载工具)
* qBittorent(种子下载工具)

## PDF
* SumatraPDF(轻量级pdf阅读软件)
* Adobe Acrobat DC(专业pdf工具)

## 绘图软件
* PPT
* Visio
* AxGlyph(实用科研矢量绘图工具)

## 英文写作
* Academic Phrasebank(论文写作语法指南)
* DeepL(翻译)
* grammarly/ginger(语法错误)
* Quillbot(同义词段落替换,更为地道)
* Thesaurus(高级词汇)

> deepl+grammarly/ginger+Quillbot+改写/参考Academic Phrasebank

## 科研
* texlive
* excel2tex插件
* zotero(文献管理)
> 插件
>
> * betterbibtex参考文献管理
>
> * Jasminum中文文献管理
>
> * ZotFile文献重命名和移动插件
>
> * zotero-doi-manager自动获取doi

* Axmath、mathtype、mathpix(公式相关)
* xtranslator，copytranslate，QTranslate(翻译论文)

## 日用软件
* clash(科学上网)
* foxmail(邮件管理)
* 7Zip(压缩与解压)
* copy++(删除复制时的换行)
* infekt nfo viewer(NFO文件阅读器)
* rufus(启动盘制作工具)

## 开发软件
### 环境管理
* docker(容器)
* wsl(windows下linux虚拟机)
* conda(python环境管理)
* MSYS2(windows下linux环境)
* Vmware/VirtualBox/EXSI(虚拟机管理)
### 后端
* Navicat(数据库可视化)
* RDM(Redis可视化)
* Postman(接口测试)
* JetBrains全家桶(IDE)
### 通用
* Zeal(文档管理)
* FileZilla Client(ftp,sftp工具)
* vscode(编辑器)
* Node.js
* curl/wget(命令行下载工具)foobar2000
* Windows terminal
* Neovide(一款由Rust开发的neovim的前端软件)
* Copilot(人工智能代码补全)
### 代码管理
* git(代码版本管理)
* Sphinx(Python文档管理工具)
* reStructuredText(Python文档格式)

## 系统相关
* rufus(启动盘制作)
* Dism++(系统优化)
* powertoys(微软开发实用工具)
* geek(软件卸载)

## 硬件检测
* AIDA64(专业全硬件检测工具)
* HWiNFO64(专业全硬件检测工具)
* CPU-Z
* GPU-Z
* CrystalDiskInfo
* MSI Afterburner(微星小飞机,可显示帧率)

## 性能测试
* Prime95(CPU烤鸡工具)
* AIDA64(包含CPU烤鸡工具)
* LinX(CPU烤鸡工具)
* FurMark(GPU烤鸡工具)
* DiskMark(硬盘速率检测工具)

## 性能优化
* BIOS超频
* ThrottleStop(CPU降压锁频超频工具)
* XTU(Intel官方性能控制软件,降压锁频超频)
* MSI Afterburner(显卡超频)
* AMD Ryzen Master(AMD cpu超频软件)
* AMD Software:Adrenalin Edition(AMD GPU超频工具)

# 常用软件总结(Linux篇)
## 办公
* wps全家桶
> 对于arch而言，有两个源cn和国际版，国际版没有云同步功能，资源占用更低

## 音视频
* vlc
* mpv
* ffmpeg(视频处理开源项目)
* deadbeef(播放器)
* osd-lyrics(歌词查找和展示器)
* kid3(歌曲tag编辑器)
* spotify(在线音乐)
* mpd + mpc + ncmpcpp(音乐播放器守护程序+接口+命令行GUI)

## 文件类
* fzf(开源文件搜索工具)
* ripgrep(开源面向行的搜索器)
* pandoc(文件类型转换)
* dolphin(文件夹工具)

## 下载
* Motrix(多线程下载工具)
* qBittorent(种子下载工具)

## PDF
* okular(pdf阅读软件)

## 绘图软件
* drawio

## 系统工具
* iwd(无线网连接)
* systemd-networkd(有线网连接)
* systemd-resolved(dns服务器)
* ntfs-3g(ntfs支持)
* fcitx5/fcitx5-chinese-addons/fcitx5-pinyin-moegirl/fcitx5-material-color(输入法/中文输入/萌娘百科词库/输入法主题)
* mesa lib32-mesa vulkan-intel lib32-vulkan-intel(intel显卡驱动)
* nvidia nvidia-settings lib32-nvidia-utils nvidia-prime(nvidia显卡驱动)
> nvidia-prime负责双显卡管理
>
> * prime-run %command% 使用nvida显卡运行命令
>
> * steam需要添加`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME="nvidia" __VK_LAYER_NV_optimus="NVIDIA_only" %command%`
* ALSA/PulseAudio(声音管理)
> ALSA是驱动级应用，早期并不支持多音频播放，之后集成了alsamixer工具之后可进行多音频播放
>
> PulseAudio是运行在ALSA之上的多音频管理器
* cups(打印服务)
> wps打印不可用时要下载cups
* zsh/ohmyzsh/powerlevel10k(zsh/优化/主题)
* p7zip/rar/unrar/zip/unzip/tar/Unarchiver(压缩和解压工具)
* rsync(拷贝和同步命令)
* wget/curl(下载/模拟http请求)
* sddm(开机显示)
* tldr(命令提示工具)

* xorg-server,xorg-setxkbmap,xorg-xsetroot,xorg-xrandr,xorg-xinit
* dunst/libnotify(通知服务器/通知显示)
* picom(合成器)
* Alacritty(支持GPU加速的终端)
* rofi(启动器)




## 日用软件
* clash(科学上网)
* lattle(docker栏)
* ark(压缩与解压)
* osd(录屏)
* wine,winetricks(windows模拟层)
* mentohust(校园网连接)
* feh(图片查看和壁纸配置)
* flameshot(截图工具)
* kvm，qemu,libvirt,virt-manager,virtio(虚拟机)
![虚拟机](/常用软件/kvm-qemu-libvirt-virtmanager.png "虚拟机架构")

## 硬件检测
* CPU-X
* GPU-Viewer
* smartmontools(硬盘信息)
* dmidecode(全面的硬件信息)

## 性能优化
* BIOS超频
* cpupower(cpu性能调整)
* intel-undervolt(cpu降压工具)
## 游戏
> 本质上是通过vulkan(一种多媒体图形接口)来运行DirectX游戏
* proton
* lutris
* dxvk(将dx转换为vulkan的转换层，使得wine可以在linux下运行3D游戏)
* mongohub(帧数显示)

