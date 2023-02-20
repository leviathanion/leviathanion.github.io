# Linux配置

# Linux配置
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

