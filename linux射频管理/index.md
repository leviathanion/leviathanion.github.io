# Linux射频管理

# Linux射频管理
## `rfkill`介绍
* `rfkill`命令可以用来打开和关闭WiFi、蓝牙等射频开关。
* 射频（rf）是Radio Frequency的缩写，`rfkill`可以管理wifi、wlan、bluetooth、uwb、wimax、wwan、gps、fm、nfc无线信号。
这种开关在某种程度上能够控制硬件的状态，使用的例子有：飞行模式、硬件节能。
## 命令
```shell
# 罗列出所有的无线设备
rfkill list

# 关闭所有的射频设备
rfkill block all
# 打开所有的射频设备
rfkill unblock all

# 可以关掉/打开某种类型的设备，例如WiFi
rfkill block wifi
rfkill unblock wifi

# 也可以对于某个设备进行打开和关闭
# 下面的编号可以从`rfkill list`中查看
rfkill block/unblock 编号
```

