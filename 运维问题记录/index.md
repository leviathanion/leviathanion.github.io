# 运维问题记录

# 运维问题记录

## 多网卡路由配置
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
  
## 设置开机启动自动运行的脚本
旧的版本中可以直接编辑 rc.local 添加开机启动脚本，而新版本这个功能默认是禁用的
* Ubuntu20.04按下操作开启rc-local.service
  * 给`rc.local`文件执行权限`chmod +x`或者`chmod 755`
  * `vi /lib/systemd/system/rc-local.service`添加如下代码
    ```shell
    [Install]
    WantedBy=multi-user.target
    ```
  * 启动服务`systemctl enable rc-local`
* Ubuntu18.04使用`rc.local`来对文件和服务命名,因此基本只需将20.04命令中的`rc-loca`改为`rc.local`即可
  * 给`rc.local`文件执行权限`chmod +x`或者`chmod 755`
  * `vi /lib/systemd/system/rc.local.service`,添加如下代码
    ```shell
    [Install]
    WantedBy=multi-user.target
    Alias=rc-local.service
    ```
  * 启用服务`systemctl enable rc.local.service` 
## ubuntu驱动内核更新导致nvidia驱动失效解决

* `sudo apt-get autoremove --purge nvidia-*`删除nvidia相关包
* `sudo apt-get install linux-headers-$(uname -r)`安装新内核的linux-headers
* `sudo apt-get install nvidia-drivers-4**`安装新的nvidia驱动

[^参考链接]:https://netplan.io/examples/

