# linux配置

# linux配置
## 网络配置
### 缺少ifconfig等工具时
* 下载net-tools包
> 目前iproute2已逐渐取代net-tools工具包，成为系统自带的网络工具，iproute2命令包主要是以ip作为前缀的一些命令
### 简单路由配置
* 显示当前路由 `route -n`
* 添加一条路由`route add -net *.*.*.* gw *.*.*.* netmask *.*.*.* dev eth0`
* 删除一条路由`route del -net *.*.*.* gw *.*.*.* netmask *.*.*.* dev eth0`

### 多网卡路由配置
> 可查询此链接[linux网络命令](http://linux-ip.net/html/index.html)查看linux网络命令的使用

* 使用策略路由

* 通过配置双路由表来配置不同网域使用不同路由表

* 具体方法

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

* 或者使用netplan[^参考链接]配置，具体

  * 修改`/etc/netplan/*.yaml`文件，示例如下

  ```yaml
  network:
      version: 2
      renderer: networkd
      ethernets:
          ens3:
              addresses:
               - 192.168.3.30/24
              dhcp4: no
              routes:
               - to: 192.168.3.0/24
                 via: 192.168.3.1
                 table: 101
              routing-policy:
               - from: 192.168.3.0/24
                 table: 101
                 priority: 10
              nameservers: 
                address: [192.168.3.1]
              gateway4: 192.168.3.1
          ens5:
              addresses:
               - 192.168.5.24/24
              dhcp4: no
              routes:
               - to: 0.0.0.0/0
                 via: 192.168.5.1
                 table: 102
               - to: 192.168.5.0/24
                 via: 192.168.5.1
                 table: 102
              routing-policy:
               - from: 192.168.5.0/24
                 table: 102
                 priority: 10
  ```

> 当设置了gateway4之后将在默认的default表自动设置对应的默认路由
> 
> 当render设置为NetWorkManager时,可能会提示不能设置没有默认路由的路由表,将render一行删除即可
>
> 缺省render的情况下，默认使用systemd-networkd
>
> 在改好相关配置后，需要systemctl restart相关服务
### cli工具
* NetWorkManager使用的cli工具是nmcli，服务是NetWorkManager.service
* systemd-networkd使用的cli工具是networkctl，服务是systemd-networkd.service
  
## 设置开机启动自动运行的脚本
旧的版本中可以直接编辑`/etc/rc.local`添加开机启动脚本，而新版本这个功能默认是禁用的
* Ubuntu20.04按下操作开启rc-local.service
  * 给`/etc/rc.local`文件执行权限`chmod +x`或者`chmod 755`
  * `vi /lib/systemd/system/rc-local.service`添加如下代码
    ```shell
    [Install]
    WantedBy=multi-user.target
    ```
  * 启动服务`systemctl enable rc-local`
* Ubuntu18.04使用`rc.local`来对文件和服务命名,因此基本只需将20.04命令中的`rc-local`改为`rc.local`即可
  * 给`rc.local`文件执行权限`chmod +x`或者`chmod 755`
  * `vi /lib/systemd/system/rc.local.service`,添加如下代码
    ```shell
    [Install]
    WantedBy=multi-user.target
    Alias=rc-local.service
    ```
  * 启用服务`systemctl enable rc.local.service` 
## 驱动
### 内核更新后重新安装ubuntu驱动
* `sudo dpkg --list | grep nvidia-*`或者`cat /proc/driver/nvidia/version`查看gpu驱动版本
* `sudo apt-get autoremove --purge nvidia-*`删除nvidia相关包
* `sudo apt-get install linux-headers-$(uname -r)`安装新内核的linux-headers，用于编译各种内核模块
* `sudo apt-get install nvidia-drivers-4**`安装新的nvidia驱动

## ubuntu更新内核
* `uname`或`hostnamectl`查看内核版本
> 可使用`uname --help`查看具体用法
* `dpkg --get-selections | grep linux`或者` dpkg --list |grep linux`查看已安装的内核版本
* `sudo apt-get install linux-image-version-generic`安装Linux镜像
* `sudo apt-get install linux-image-extra-version-generic`安装新内核的额外驱动
* `sudo apt-get install linux-headers-version-generic`安装linux-headers
* `sudo apt-mark hold linux-image-version-generic`固定内核镜像
* `sudo apt-mark hold linux-image-extra-version-generic`固定内核的额外驱动
* `sudo apt-mark hold linux-headers-version-generic`固定linux-headers

## grub相关
* 开机时按`shift`可进入grub界面
* `sudo vim /etc/default/grub`修改`grub`文件可修改默认启动项
* 修改完之后要用`sudo update-grub`来更新grub
* ` /boot/grub/grub.cfg `包含了各个启动项的详细信息
* 若重装系统时，开机出现引导失败，进入grub rescue模式，可尝试写入启动盘时使用dd模式

### ubuntu图形界面失效
* `ctrl+alt+F2~F6`可进入tty命令行界面
* 如果出现图形界面黑屏，但可以进入tty模式，考虑如下方法解决
    * 修改`/boot/grub/grub.cfg`，在`quiet splash`后面添加`nomodeset`
    * 修改`/etc/default/grub`,在`quiet splash`后面添加`nomodeset`
## 用户管理
* 涉及四个文件，主要需要修改passwd文件
| 文件| 含义 |
|  :--:  |  :--:  |
|/etc/shadow |加密的用户账户信息 |
|/etc/passwd |用户账户信息 |
|/etc/gshadow |隐藏的组账户信息 |
|/etc/group |定义了用户属于哪个组 |
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

* `userdel`命令，删除用户，加`-r`参数也删除用户主目录
* `usermod`命令，参数与`useradd`一致，含义变为修改
> 修改`/etc/sudoers`文件，为用户添加`sudo`权限，添加如下
> ```shell
> username ALL = (ALL) ALL
> ```

## 安全相关
### SSH安全
* 具体参阅archwiki

### ufw防火墙设置
* `sudo ufw status`查看防火墙状态
* `sudo ufw default deny`默认拒绝
* `sudo ufw allow from 192.168.0.0/24`允许某个ip段访问
* `sudo ufw all 22/tcp/udp`允许22端口的tcp或者udp访问，若不加tcp或者udp则都允许访问
* `sudo ufw all ssh`允许ssh的端口访问
* `sudo ufw limit ssh`限制ssh的访问，禁用过去30秒尝试启动6个或以上的IP连接
* `\etc\ufw\user.rules`存储了规则，`\etc\ufw\before.rules`可以设置黑名单
> 详细可参阅官方文档

## nvim配置
### linux
* 从github拉取配置文件并放在.config目录下，命名为nvim
```shell
git clone https://github.com/leviathanion/nvim-config.git  .config/nvim
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

[^参考链接]:https://netplan.io/examples/
[^packer]:https://github.com/wbthomason/packer.nvim

