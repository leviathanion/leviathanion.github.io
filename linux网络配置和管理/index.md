# Linux网络配置和管理

# 网络管理
> 可查询此链接[linux网络命令](http://linux-ip.net/html/index.html)查看linux网络命令的使用
## 网络检查步骤
1. `ip link show`查看网卡情况
2. `ip addr show`查看ip情况
3. `networkctl`查看网络接管情况
4. `resolvectl`查看dns情况
5. `ping baidu.com`查看网络最终情况

## `systemd-resolved`配置
* 自动管理dns配置: `ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf`
    * 配置文件中包含了上级dns服务器及搜索域名
* 手动dns文件配置在两个地方`/etc/systemd/resolved.conf`或者`/etc/systemd/resolved.conf.d/*`
    * 在`/etc/systemd/resolved.conf.d/dns_servers.conf`中配置
    ```
    [Resolve]
    DNS=114.114.114.114
    Domains=~.
    ```
    * 在`/etc/systemd/resolved.conf.d/fallback_dns.conf`中配置
    ```
    [Resolve]
    FallbackDNS=8.8.8.8
    ```
    如果要禁用fallback_dns 功能,则不设置FallbackDNS参数
* 上述配置等效于`在/etc/systemd/resolved.conf`将二者一起配置

## `systemd-networkd`配置
* 需要在`/etc/systemd/network/自定义.network`中自己配置
```
[Match]
Name=enp1s0 # 也支持 en* 的正则表达式
[Network]
DHCP=yes/ipv4/ipv6
DHCPServer=
DNS=
Address=
Gateway=
Domains=
[DHCP]/[DHCPv4]/[DHCPv6]
RouteMetric=
```
## `iwd`无线网配置
* `iwctl`命令进入wifi连接界面
* `station wlan0 get-network`查看目前wifi情况
* `station wlan0 scan`扫描wifi
* `station wlan0 connect wifi名`连接wifi

## 多网卡路由配置

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

## cli工具
* NetWorkManager使用的cli工具是nmcli，服务是NetWorkManager.service
* systemd-networkd使用的cli工具是networkctl，服务是systemd-networkd.service

