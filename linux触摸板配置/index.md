# Linux触摸板配置

# Linux触摸板配置
## libinput
`libinput`是一个函数库，在 Wayland 上用来接收设备的输入，在 X.Org 上提供输入设备的驱动。它提供对设备事件的检测和接收。对输入设备信号进行处理。它提供了一些列的函数供用户使用。
## 安装
* `Wayland`下不需要手动安装
* `Xorg`下需要安装`xf86-input-libinput`,如果需要程序运行时改变配置,则需要安装`xorg-xinput`
## 配置
`Wayland`下没有`libinput`的配置文件,不同桌面具有不同的支持

`Xorg`默认配置文件位于`/usr/share/X11/xorg.conf.d/40-libinput.conf`中

```shell
libinput list-devices
```
可以查看哪些设备被libinput支持,同时也可以使用以下命令
```shell
grep -e "Using input driver 'libinput'" /path/to/Xorg.0.log
```
### 在`40-libinput.conf`中配置
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
        Option "Tapping" "on"
        Option "NaturalScrolling" "on"
        Option "TappingButtonMap" "lrm"
EndSection
```
* 配置文件中存在多个`Section`时,需要配置`Match*`,这通常代表着过滤
    * MatchIsPointer "on" (trackpoint)
    * MatchIsKeyboard "on"
    * MatchIsTouchpad "on"
    * MatchIsTouchscreen "on"
* 常用配置选项如下
    * Option "Tapping" "on": 触摸以点击
    * Option "ClickMethod" "clickfinger": 触摸板不再拥有中右键区域的区分，与之代替的是双指代表右键，三指代表中键。 详情请看docs.
    * Option "NaturalScrolling" "on": 自然滚动（反方向滚动）
    * Option "ScrollMethod" "edge": 边缘滚动页面
    * Option "TappingButtonMap" "lrm": 单指,两指,三指分别代表鼠标左,右,中的点击
### `30-touchpad.conf`中配置
* 配置与上述一致,但只需配置一个`Section`
## 触摸板手势
* 安装`libinput-gestures`和`xdotool wmctrl`
* 将当前用户添加到`input`组里:`sudo gpasswd -a $USER input`
* 拷贝配置文件到`XDG_CONFIG_HOME`:`cp /etc/libinput-gestures.conf ~/.config/`
* 修改`libinput-gestures.conf`配置文件格式为: `gesture 动作 映射`
* 动作如下
    * `swipe up/down/left/right n`:n指向上下左右滑动
    * `pin in/out n`:n指捏张
    * `swipe right_up/right_down/left_up/left_down n`:n指向右上,右下,左上,左下滑动
    * `pin clockwise/anticlockwise n`:n指顺时针,逆时针转动
* 映射可使用`xdotool`映射: `xdotool key 按键`,按键支持`x keysym`风格的字符串,可以通过`xev`获取

```
gesture swipe left 4 xdotool key super+h # 4指左划: 切换到左侧显示器
gesture swipe right 4 xdotool key super+l # 4指右划: 切换到右侧显示器
gesture swipe up 3 xdotool key super+0 # 指下划: 浮动切换
```
* `libinput-gestures-setup autostart`后`libinput-gestures-setup start`启动



