# Linux电源管理

# Linux电源管理
## 亮度控制
### 硬件控制
硬件控制主要通过ACPI内核模块的接口来控制
`/sys/class/backlight/`提供了亮度控制的接口
* `/sys/class/backlight/显卡名/`下包含多个文件
    * `max_brightness`表示最大亮度
    * `brightness` 表示当前亮度,可以修改它来修改亮度
### 软件控制
#### DPMS
DPMS可以在计算机不使用时启用显示器的节能行为
##### Xorg配置文件设置DPMS
* 在`/etc/X11/xorg.conf.d/`或者`/usr/share/X11/xorg.conf.d/`中文件的`Monitor`中添加
```
Option "DPMS" "true"
```
* 在`ServerFlags`中添加
```
Option "StandbyTime" "10"
Option "SuspendTime" "20"
Option "OffTime"    "30"
```
> 如果"OffTime"无效,则使用"blackTime"来代替

* 需要禁用时,将`DPMS`设置为`false`,其他选项设置为`0`
##### `xorg-xset`命令行设置DPMS
* 查询当前设置:`xset q`
* 启动DPMS:`xset +dpms`
* 关闭DPMS:`xset -dpms`
* 关闭屏保:`xset s off`
* 设置屏保:`xset s x`,经过x秒进入屏保
* 设置参数:`xset dpms x x x`,三个参数分别对应StandbyTime,SuspendTime,OffTime
#### `xorg-xbacklight`
* 设置亮度为50%:`xbacklight -set 50`
* 设置亮度增加10%:`xbacklight -inc 10`
* 设置亮度减少10%:`xbacklight -dec 10`
如果报错`No outputs have backlight property`,
* xbacklight没有选择到正确的ACPI接口目录,需要手动设置
在`/etc/X11/xorg.conf.d/20-video.conf`或者`/usr/share/X11/xorg.conf.d/20-video.conf`中如下设置
```
Section "Device"
    Identifier  "Intel Graphics"
    Driver      "intel"
    Option      "Backlight"  "intel_backlight"
EndSection
```
* 开启了`Intel Fastboot`导致的问题
#### `light`
* 将用户添加到`video`组里
* 设置亮度为50%:`light -S 100`
* 设置亮度增加10%:`light -A 10`
* 设置亮度减少10%:`light -U 10`
## cpu频率控制
* 安装`cpupower`
* 查看频率信息:`cpupower frequency-info`
* 设置最大频率:`cpupower frequency-set -u xGHz`
* 设置最小频率:`cpupower frequency-set -d xGHz`
* 设置固定频率:`cpupower frequency-set -f xGHz`
> `-c core_number`指定cpu核心
* 设置睿频:`echo 0 > /sys/devices/system/cpu/cpufreq/boost`
* 设置预设值:`cpupower frequency-set -g performance/powersave`
* 设置EPB:`cpupower set -b epb_value`,值的范围是`0-15`,越大越节能
## TLP软件
* `TLP`一款`Linux`省电工具
### 配置
* `/etc/tlp.conf`或者`/etc/tlp.d/*.conf`
* 设置总体调度策略
```
CPU_SCALING_GOVERNOR_ON_AC=powersave
CPU_SCALING_GOVERNOR_ON_BAT=powersave
```
* 设置CPU 的能源策略，在电池模式下省电，而在外部电源供电模式下偏向性能：
```
CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
CPU_ENERGY_PERF_POLICY_ON_BAT=power
```
* 设置电池禁止睿频,插电允许
```
CPU_BOOST_ON_AC=1
CPU_BOOST_ON_BAT=0
```
* 设置电池自动缩减核心数,插电禁止
```
SCHED_POWERSAVE_ON_AC=0
SCHED_POWERSAVE_ON_BAT=1
```
* 设置USB自动挂起
```
USB_AUTOSUSPEND=1
```
* 启动`TLP`:`sudo systemctl enable tlp.service`

