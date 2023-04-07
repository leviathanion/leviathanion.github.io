# Linux桌面环境和窗口管理器

# Linux桌面环境和窗口管理器
## Desktop Environment(DE)[^1]
> 一般情况下DE包含WM，比如Gnome3的默认WM是Mutter，KDE5的默认WM是KWin。主流的桌面环境都提供了大量自带的桌面组件来构成其完整的桌面体验。
>
> 桌面环境通常提供的组建包括
> * 桌面合成器(Xorg)
> * 窗口管理器
> * 通知守护进程
> * 显示管理器(开机之后要求输入密码的登陆界面SDDM等)
> * 屏幕锁定器
> * 注销管理
> * 应用启动器
> * 音频控制
> * 亮度控制
> * 电源管理器
> * 屏幕截图
> * 壁纸
> * 默认应用程序
> * 媒体控制器
> Wayland上桌面合成器和窗口管理器是同一个东西


|环境|介绍|优点|缺点|
| ---| ---| ---| ---|
| GNOME | GNOME（/ɡˈnoʊm/）是一个完全由自由软件组成的桌面环境。它的目标操作系统是Linux，但是大部分的BSD系统亦支持GNOME。 | 1.简单易用, 2.可通过扩展插件支持非常丰富的功能 | 1.耗用内存、CPU资源较多; 2.插件安装与管理略显繁琐，需要通过网络浏览器下载。 |
| Unity | 一款与Ubuntu分分离离的桌面环境，最初是2011年支持Ubuntu的商业公司Canonical Ltd开发的，但在2017年的Ubuntu 17.10放弃Unity7而选择了GNOME，但在2020年的Ubuntu 20.04又重新启用了Unity7，并且起名为Ubuntu Unity，而且令人惊讶的是让Unity7能够起死回生的开发者是一位10岁的少年，他的名字叫Rudra Saraswat。|与GNOME一样的易用，少量的定制选项，让我们可以开箱即用。|并没有解决GNOME的缺点，插件管理依然需要一个稍微繁琐的步骤来实现安装|
|Cinnamon|与Unity一样，它也是GNOME的亲兄弟，Linux Mint团队因对GNOME3的改进无法接受，因而Fork出了一个分支，这就是Cinnamon，它的目标是让用户可以获得开箱即用用户界面。|1.界面非常优美，简单易用; 2.内置配置管理器可以非常方便的管理插件、主题、小工具等； 2.丰富的选项让定制变得非常简单|要说缺点就是BUG可能会稍多一些，而且2016年2月20日，未知黑客入侵Linux Mint网站也让用户对其安全性也比较担心(虽然此事与Cinnamon并无关系)。|
|MATE|MATE(/ˈmɑːteɪ/)桌面环境是 GNOME 2 的延续。|1.轻量级的GNOME，资源占用少; 2.适合老电脑、配置低的电脑设备;|正因为继承自GNOME 2，对于很多GNOME 3的新功能已经无法支持，但是主流系统还是会提供适配MATE桌面环境的系统，例如Ubuntu MATE|
|KDE Plasma|一款庞大而复杂的桌面环境，使用Qt开发，开发社区一直以来都是非常活跃，并且没有想GNOME有很多的分支，界面开发接口非常统一。|功能非常强大、高度可定制，更加现代化的用户界面。|1.正因为功能强大，带来的复杂性也使得普通用户使用起来有些不知所措; 2.漂亮的外观也带来了更多的内存和CPU资源的占用。|
|Xfce|一个轻量级的桌面环境，与GNOME一样，Xfce也基于GTK工具包开发，目标是快速轻巧，同时在视觉上仍然有吸引力并且易于使用。|1. 轻量级; 2.Xfce遵守标准，尤其是在freedesktop.org上定义的标准; 3.内存和CPU资源占用非常少(非常适合老笔记本、嵌入式设备等)|1.外观上有些简陋; 2.内置应用很少。|
|LXDE|使用GTK工具包编写，旨在提供一个快速且节能的桌面环境。所以与Xfce非常相似。主要活跃地区为台湾地区。|1.轻量级。|1.外观简陋。|
|LXQt|一款GTK和Qt融合的桌面环境，产生原因是LXDE团队不满于GTK+3的不向下兼容等诸多问题，因而转向了Qt框架。与LXDE一样，主要活跃地区为台湾地区。截至2020年4月24日的稳定版为0.15.0。|1.轻量级。|1.外观简陋。|
|DeepinDE|深度系统（Deepin）的桌面环境，由中国的武汉深之度科技有限公司(简称深度科技)为主要开发厂商。它使用Qt开发。|1.非常漂亮的用户界面; 2.内置很多Windows平台应用软件，更适合中国用户的使用习惯。|1.资源占用较多; 2.使用过程中的BUG较多，不过都在被逐步解决。|
### 追求华丽，硬件配置较高
* KDE
* GNOME
* DeepinDE
### 配置较低
* Xfce
* LXDE
* LXQt
## Windows Manager(WM)
> 窗口管理器只是管理出现在你的桌面上的各种窗口的组件，具体的管理内容包含比如：窗口堆叠方式，窗口移动规则等。

### Xorg[^2]
* 堆叠式(大多数桌面发行版使用的都是堆叠式窗口管理器，窗口可以互相堆叠)
* 平铺式(窗口没有重叠部分)
    * BspWM
    * i3wm(c语言编写,perl配置)
* 动态(可以在堆叠式和平铺式之间进行切换)
    * DWM(C语言)
    * AwesomeWM(DWM的扩展，使用lua语言配置)
    * Xmonad(haskell编写)
    * Qtile(Python编写)
### Wayland[^3]
* 堆叠式
    * wayfire
    * 各大发行版自带
* 平铺式
    * Sway(i3)
    * Qtile
    * dwl(dwm)

### 之后的配置
* 由于WM缺少很多桌面环境下需要的东西，因此需要手动安装，所有的这些组件都可以在[^1]中找到对应的arch wiki目录
    * 通知管理[^4]：libnotify+dunst(libnotify提供通知,dunst作为通知服务器)
    * 音量调节[^5][^6]:安装alsa(pulseaudio/pireware)后使用amixer/alsamixer(pactl/pulsemixer)(alsa和pulseaudio的工具)进行管理(前者命令行,后者提供命令行ui,tui)
    * 亮度调节[^7]:xbacklight/light/acpilight(使用mesa驱动的intel核显无法使用xbacklight)或直接修改`/sys/class/backlight/intel_backlight/brightness`的值
    * 顶部窗口:slstatus(xorg),waybar(wayland)
    * 显示管理器:SDDM(xorg)
    * 窗口特效:picom(xorg)
    * 应用启动器:dmenu/rofi(xorg),bemune/wofi(wayland)
    * 媒体控制器[^8]:MPRIS+控制组件(Playerctl/mpris-player-control/D-Bus)
    * 锁屏器:各大WM有对应的锁屏器
    * 截图工具:flameshot
    * 壁纸:feh



[^1]:https://wiki.archlinux.org/title/desktop_environment
[^2]:https://wiki.archlinux.org/title/window_manager
[^3]:https://wiki.archlinux.org/title/Wayland
[^4]:https://wiki.archlinux.org/title/Desktop_notifications
[^5]:https://wiki.archlinux.org/title/List_of_applications#Volume_control
[^6]:https://wiki.archlinux.org/title/PulseAudio#Front-ends
[^7]:https://wiki.archlinux.org/title/Backlight
[^8]:https://wiki.archlinux.org/title/MPRIS#Control_utilities

