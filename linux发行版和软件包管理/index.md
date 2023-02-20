# Linux发行版和软件包管理

# Linux发行版和软件包管理
## Linux发行版和常规包管理
Linux发行版虽然很多，但是可以根据软件安装包的格式进行大体上的分类，这样类似的系统使用方法都没有太大差异的。根据软件安装包的格式可以将Linux发行版分为以下四类，根据包管理器的不同又可以进一步细分(括号前为包管理器名称，括号里为常用指令):
* `.deb`格式，使用此类安装包的系统通常派生自`Debian`系统，我们将这类系统划分为`Debian系列`。
    * `DPKG(dpkg/apt)`:Debian,Ubuntu,LinuxMint,Kali
* `.rpm`格式，使用此类安装包的系统通常派生自`Red Hat`系统，我们将这类系统划分为`Red Hat系列`。
    * `RPM(rpm/yum)`:RedHat,Centos
    * `DNF(dnf)`:Fedora
    * `Zypper(zypper)`:openSUSE
* `.pkg.tar.xz`格式，使用此类安装包的系统通常派生自`Arch`系统，我们将这类系统划分为`Arch系列`。
    * `Pacman(pacman)`:Arch,Manjaro,Artix
* 从源码构建，使用此类形式的系统通常派生自`Gentoo`系统，我们将这类系统划分为`Gentoo系列`。
    * `Portage(emerge)`:Gentoo
> apt为dpkg的前端。yum/dnf/zypper为rpm的前端
> * 通常使用dpkg和rpm命令来管理deb文件和rpm文件
> * 使用apt,yum/dnf/zypper从源中下载对应的包
> 
> 命令的使用方法可以使用`--help`或者`man`参数来查询
## 特立独行的软件安装方法
上面列举的软件包管理工具都是重点考虑如何解决软件包依赖问题，而有些软件安装方法就不需要这种考虑，这类软件被称为`(portable software)便携软件`，在Windows系统中被称做`绿色软件`，这类软件不需要安装就可以直接使用。常见的`便携软件`格式如下:
* `AppImage`:核心思想是一个应用程序 = 一个文件，下载即用，非常适合无需root权限的软件。
    * 为其附加执行权限(参考Linux权限管理)
    * `./*.AppImage`运行
* `Flatpak`:口号是Linux系统上的软件的未来，Flatpak的目标是在用户想要运行他们可能并不完全信任的应用软件时提供一个安全的沙盒环境供用户使用。应用程序将必须使用由flatpak提供的函数调用来控制硬件设备或访问用户的文件，而flatpak将会在给予应用程序访问权限前提示用户。Flatpak允许应用程序开发人员直接向用户提供更新，而无需通过发行版，而不必为每个发行版分别打包和测试应用程序。提升了软件更新的速度但也可能会降低稳定性。
    * 使用`flatpak`命令管理[^1]
* `Snappy`:snap格式包是一种可以由主机操作系统动态挂载的压缩的文件系统，其中还附有元数据声明，snap系统可以据其为应用程序设置适当的安全沙箱或容器。安装snap软件后执行df命令，你可以看到多了/dev/loopX文件系统。
    * 使用`snap`命令管理[^2]

[^1]:https://docs.flatpak.org/zh_CN/latest/using-flatpak.html
[^2]:https://cn.ubuntu.com/blog/what-is-snap-application

