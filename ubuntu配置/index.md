# Ubuntu配置

# Ubuntu配置
## 网络配置
* 缺少ifconfig等工具时，下载net-tools包(不建议使用)
> 目前iproute2已逐渐取代net-tools工具包，成为系统自带的网络工具，iproute2命令包主要是以ip作为前缀的一些命令
* 使用netplan[^参考链接]配置，具体
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
> ubuntu中的开机脚本使用systemd来配置，但配置文件不完全遵守systmed规范
>
> 规范的systemd配置请参考博客中的Linux配置
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
## 内核更新后重新安装nvidia驱动
### 方法1:通过apt安装
* `sudo dpkg --list | grep nvidia-*`或者`cat /proc/driver/nvidia/version`查看gpu驱动版本
* `sudo apt-get autoremove --purge nvidia-*`删除nvidia相关包
* `sudo apt-get install linux-headers-$(uname -r)`安装新内核的linux-headers，用于编译各种内核模块
* `sudo apt-get install nvidia-drivers-4**`安装新的nvidia驱动
### 方法2:通过dkms安装
* `nvcc -V`查看有cuda驱动
* `ls /usr/src | grep nvidia`查看之前nvidia驱动版本
* `sudo apt install dkms`安装dkms
* `sudo dkms install -m nvidia -v 驱动版本`安装nvidia驱动

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

## 图形界面失效
* `ctrl+alt+F2~F6`可进入tty命令行界面
* 如果出现图形界面黑屏，但可以进入tty模式，考虑如下方法解决
    * 修改`/boot/grub/grub.cfg`，在`quiet splash`后面添加`nomodeset`
    * 修改`/etc/default/grub`,在`quiet splash`后面添加`nomodeset`
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

[^参考链接]:https://netplan.io/examples/
[^packer]:https://github.com/wbthomason/packer.nvim

