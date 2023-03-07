# Linux配置

# Linux配置
## 文件系统层次概述
文件系统包含了很多不同的初始路径，本节对不同的路径名称进行概述，可以通过`man file-hierarchy`来查看详细信息
### 通用目录
* `/`:根目录
* `/boot/`:存放操作系统的启动的核心文件，应考虑设置为只读。
* `/etc/`:存放系统特定的配置文件和子目录
* `/home/`:存放用户家目录，除系统用户外，每个用户应该有一个以自己用户名为名的家目录，可以通过`$HOME`访问
* `/root/`:`root`用户的家目录
* `/srv/`:存放服务启动后需要的数据
* `/tmp/`:存放小的临时文件
    * 通常被挂载为`tmpfs`
    * 大的临时文件存放在`/var/tmp`
    * 在启动时被刷新
    * 文件没有确定的删除时间
* `/mnt/`:用户挂载目录
* `/lost+found/`:通常为空，非正常关机时存储一些文件
* `/bin/`:存放二进制文件
* `/sbin/`:存放超级用户使用的二进制文件
* `/lib/`:存放动态共享库，类似于windows的dll文件
* `/lib64/`:同上
* `/opt/`:用户级的程序目录
    * 类似于`D:/Software`
    * `/usr/local/`类似于`C:/Program Files/`，自己编译的软件会安装到此`/usr/local/`
> 注意bin,sbin,lib,lib64需要查看兼容性符号链接章节
### 运行时目录
* `/run/`:存放运行时数据，套接字文件等的临时文件系统
    * 在启动时被刷新
    * `/var/run/`应指向此目录
* `/run/log/`:存放实时系统日志
* `/run/usr/`:存放每个用户的运行时目录
    * 通常每个都是个临时文件系统
    * 当用户log out 或者重启时刷新
* `/run/media/`:存放实时挂载的硬盘
### 厂商提供的资源目录
* `/usr/`:存放厂商提供的程序和文件
* `/usr/src/`:存放所有程序的源代码，主要是Linux核心程序
* `/usr/local/`:存放本地安装的软件和其他文件，与系统无关
* `/usr/bin/`:存放系统用户使用的二进制文件
    * shell不可用的二进制文件应存放到`/usr/lib/`
* `/usr/include/`:存放系统库的`c/c++ api header`
* `/usr/lib/`:静态的私有的厂商数据，放有内部可执行文件和shell不可用的二进制文件，动态共享库
* `/usr/share/`:在多个包之间共享的资源，例如文档，man pages，字体等
* `/usr/share/doc/`:操作系统或系统包的文档
### 持久性可变系统文件
* `/var/`:存放持久性的可变的系统数据
* `/var/cache/`:持久性的系统缓存数据
* `/var/lib/`:持久性的系统数据
* `/var/log/`:持久性的系统日志
* `/var/tmp/`:更大的持久实时系统，相对于`/tmp/`
### 虚拟内核和API文件系统
* `/dev/`:存放外部设备
* `/proc/`:存放着程序列表和功能性组件等的虚拟文件系统
    * 虚拟目录，是内存的映射
    * 不在硬盘而在内存上，可以修改
* `/proc/sys/`:存储内核状态
* `/sys/`:特殊的文件系统，它是内核向用户空间暴露系统信息的一种方式。它的目录结构和文件通过 sysfs 文件系统实现，是一种动态的文件系统，随着系统状态的变化而变化。
### 兼容性符号链接
* `/bin`,`/sbin`,`/usr/sbin/`->`/usr/bin`上
* `/lib/`->`/usr/lib/`
* `/lib64/`->`/usr/lib64/`
* `/var/run/`->`/run/`

## Linux基本目录规范-XDG
XDG Base Directory Specification
> 该规范定义了一套指向应用程序的环境变量，这些变量指明的就是这些程序应该存储的基准目录。而变量的具体值取决于用户，若用户未指定，将由程序本身指向一个默认目录。
### 用户层面
* `XDG_CONFIG_HOME`:存储用户特定的配置文件，默认为`$HOME/.config/`
* `XDG_CACHE_HOME`:存储用户特定非重要缓存数据，默认为`$HOME/.cache/`
* `XDG_DATA_HOME`:存储用户特定的数据文件，默认为`$HOME/.local/share/`
* `XDG_STATE_HOME`:存储用户特定的状态文件，默认为`$HOME/.local/state/`
* `XDG_RUNTIME_HOME`:用户特定的不重要的运行时文件和其他文件对象（例如套接字，命名管道…）存储的基本目录。
    * 不要求有默认值；如果没有设置或提供等价物，应发出警告。
    * 该目录必须由用户拥有，并且他必须是唯一具有读写访问权限的目录。它的Unix访问模式必须是0700。
    * 按照操作系统的标准，文件系统功能齐全。
    * 必须在本地文件系统上。
    * 可能会被定期清理。
    * 每6小时修改一次，如果需要持久性，则设置粘性位。
    * 只能在用户登录的时间内存在。
    * 不应存储大文件，因为它可能被挂载为临时文件。
    * pam_systemd将其设置为/run/user/$UID。
### 系统层面
* `XDG_DATA_DIRs`:定义了一套按照偏好顺序的基准目录集，用来搜索除了 $XDG_DATA_HOME 目录之外的数据文件。该目录中的文件夹应该用冒号（:）隔开。默认值是 /usr/local/share/:/usr/share/。
* `XDG_CONFIG_DIRS`:定义了一套按照偏好顺序的基准目录集，用来搜索除了 $XDG_CONFIG_HOME 目录之外的配置文件。该目录中的文件夹应该用冒号（:）隔开。默认值是 /etc/xdg。

## 网络管理
> 可查询此链接[linux网络命令](http://linux-ip.net/html/index.html)查看linux网络命令的使用
### `systemd-networkd`配置

### `systemd-resolved`配置

### `iwd`无线网配置

### 多网卡路由配置

* 使用策略路由，使用多个路由表来配置不同的网域路由

  * 查看路由表存储情况


  ```shell
  cat /etc/iproute2/rt_tables
  ```

  ​		输出情况如下

  ```
  #
  # reserved values
  #
  255		local		    #本地路由表
  254		main		#主路由表，不加设定时我们增加的路由规则都设置于此
  253		default		#存放默认路由规则。注意增加默认规则时若没有指定路由表那还是存在于main表中
  0		unspec
  #
  # local
  #
  #1	inr.ruhep
  ```

  * `route -n`命令查看的是`main`路由表
  * 查看对应路由表

  ```shell
  ip route show table main
  ip route show table 254
  ```

  * 修改rt_tables文件向指定路由表添加或删除规则使用`ip route`命令

  ```shell
  ip route add 192.168.80.0/24 via 192.168.20.20 table 251 
  ip route add 192.168.80.0/24 via 192.168.30.20 table 252
  ip route del 192.168.80.0/24 via 192.168.30.20 table 251
  ip route del 192.168.80.0/24 via 192.168.30.20 table 252
  ```
  * 快速删除某一特定路由或者路由表可使用`ip route flush`

  ```shell
  #清除192.168.0.0的路由信息
  ip route flush 192.168.0.0
  #清空main路由表 
  ip route flush table main 
  ```
  * 查看路由表策略

  ```shell
  # ip rule show
  或者 # ip rule ls
  ```

  ```
  0:	from all lookup local 
  32766:	from all lookup main 
  32767:	from all lookup default
  ```

  * 创建策略( pref 越小越先匹配)

  ```shell
  #根据源地址决定路由表
  ip rule add from 192.168.10.0/24  table 100 pref 10
  ip rule add from 192.168.20.20    table 110 pref 100
  
  #根据目的地址决定路由表
  ip rule add to   192.168.30.0/24  table 120
  ip rule add to   192.168.40.0/24  table 130
  
  #根据网卡设备决定路由表
  ip rule add dev  eth0  table 140
  ip rule add dev  eth1  table 150
  
  #此外还可以根据其他条件进行设置，例如tos等等
  ```

  * 删除策略

  ```shell
  #根据明细条目删除
  ip rule del from 192.168.10.10
  
  #根据优先级删除
  ip rule del prio 32765
  
  #根据表名称来删除
  ip rule del table wangtong
  ```

### cli工具
* NetWorkManager使用的cli工具是nmcli，服务是NetWorkManager.service
* systemd-networkd使用的cli工具是networkctl，服务是systemd-networkd.service
  
## 用户管理
* 涉及四个文件，主要需要修改passwd文件
| 文件| 含义 |
|  :--:  |  :--:  |
|/etc/shadow |加密的用户账户信息 |
|/etc/passwd |用户账户信息 |
|/etc/gshadow |隐藏的组账户信息 |
|/etc/group |定义了用户属于哪个组 |
### 用户添加
* `useradd`命令，可通过`man useradd`查看用法
| 参数 | 含义 |
|  :--:  |  :--:  |
| -d 目录 | 指定一个用户目录，若目录不存在，需要-m参数创建主目录 |
| -g 用户组| 指定用户所属的用户组|
| -G 用户组，用户组| 指定用户所属的附加组|
| -s shell | 指定用户登录的shell|
| -u 用户号| 指定用户的用户号|
| -m | 自动建立用户的登录目录|
| -M | 不自动建立用户的登录目录|
| -n | 不自动建立以用户名为名的用户组|
> * 新建一个普通用户
> ```shell
>  useradd -d  /home/username -m username -s /bin/bash
>  passwd username
>  ```
> * 新建一个管理员用户
> ```shell
>  useradd -d  /home/username -m username -s /bin/bash -g sudo
>  passwd username
>  ```

### 用户删除
* `userdel`命令，删除用户，加`-r`参数也删除用户主目录
### 用户修改
* `usermod`命令，参数与`useradd`一致，含义变为修改
> 修改`/etc/sudoers`文件，为用户添加`sudo`权限，添加如下
> ```shell
> username ALL = (ALL) ALL
> ```
### 用户密码管理
* `passwd 选项 用户名`
    * `-l` 锁定账号，禁止登陆
    * `-u` 解锁账号
    * `-d` 使账号无密码，同时不能登陆
    * `-f` 强迫用户下次登陆时修改密码
### 用户组管理
* `groupadd 选项 组名` 添加一个用户组
    * `-g`指定用户组编号GID
    * `-o`表示新用户组的GID可以和老用户组相同
* `groupdel 组名`删除一个用户组
* `groupmod 选项 组名`
    * 除了add之外的参数，多了一个`-n`表示重命名
* `newgrp 组名`如果一个用户同时属于多个用户组，使用此命令可切换组
## 安全相关
### SSH安全
* 具体参阅archwiki

## nvim配置
### linux
* 从github拉取配置文件并放在.config目录下，命名为nvim
```shell
git clone https://github.com/leviathanion/nvim-config.git  ~/.config/nvim
```
* 下载packer插件管理工具[^packer]，然后进入nvim运行`:PackerSync`
* 可通过`:checkhealth`查看各插件情况
* 在.bashrc或者.zshrc中添加以下代码，使得root权限也可用
```shell
alias sudonvim = "sudo -E nvim"
```
### windows
* 与linux类似，将配置文件放到`c:user/appdata/local/nvim`中

## zsh配置conda
* cd anaconda的安装目录下的bin目录
* 执行conda init zsh `conda init zsh`

[^packer]:https://github.com/wbthomason/packer.nvim

