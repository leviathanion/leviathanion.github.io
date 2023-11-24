# Linux基础概念扫盲

# Linux基础概念扫盲
## XDG
* XDG代表"Cross-Desktop Group"，是来自freedesktop[^1]的一系列规范
* 旨在标准化各类系统上的桌面和其他GUI应用程序
* 广泛采用的一些协议：
    * `Autostart`：应用程序如何在用户登录后自动启动，以及可移动媒体如何请求执行特定应用程序或在安装媒体后请求打开媒体上的特定文件。
    * `桌面基本目录(basedir)`：桌面应如何定位文件，例如配置文件或应用程序数据文件。
    * `桌面条目 (.desktop)`：描述有关应用程序信息的文件，例如名称、图标和描述。这些文件用于应用程序启动器和创建可启动的应用程序菜单。
    * `桌面菜单 (menu)`：如何从桌面条目构建菜单。
    * `文件管理器 D-Bus 接口`：与桌面文件管理器交互的常用方式。
    * `文件 URI`：如何创建和解释解释file://URI，用于拖放和其​​他桌面用途。
    * `免费媒体播放器规范`：跨播放器和媒体格式存储和读取元数据的标准方法。
    * `图标主题`：存储图标主题的常用方式。
    * `媒体播放器远程接口规范 (MPRIS)`：控制媒体播放器的 D-Bus 接口
    * `共享 MIME 数据库（shared-mime-info）`：包含常见的 MIME 类型、描述和确定文件类型的规则。
    * `启动通知`：一种允许桌面环境跟踪应用程序启动、提供用户反馈和其他功能的机制。
    * `Trash`：一种常见的方式，所有“垃圾桶”实现都应该存储、列出和取消删除已删除的文件。
    * `XML 书签交换语言 (XBEL)`：一种互联网“书签”交换格式。 
* 一些常用的协议：
    * `The MIME Applications specification`：决定了默认程序 
## XDG目录规范
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
* `XDG_DATA_DIRS`:定义了一套按照偏好顺序的基准目录集，用来搜索除了 $XDG_DATA_HOME 目录之外的数据文件。该目录中的文件夹应该用冒号（:）隔开。默认值是 /usr/local/share/:/usr/share/。
* `XDG_CONFIG_DIRS`:定义了一套按照偏好顺序的基准目录集，用来搜索除了 $XDG_CONFIG_HOME 目录之外的配置文件。该目录中的文件夹应该用冒号（:）隔开。默认值是 /etc/xdg。

## XDG应用程序入口规范
XDG Desktop Entry specification定义了一种标准，用于将应用程序集成到实现XDG桌面菜单规范的桌面环境的应用程序菜单中。
* 每个`.desktop`文件必须包括`Type`和`Name`，该规范定义了三种`Type`：`Desktop` `Link` `Directory`
    * `Desktop`应用程序类型定义了如何启动一个应用程序以及它支持的MIME类型（用于XDG MIME应用程序）。通过将应用程序条目放在特定的目录中，可以使用XDG自动启动来自动启动应用程序条目。应用程序条目使用.desktop文件扩展名。请参见#应用程序条目。
    * `Link`链接类型定义了一个指向URL的快捷方式。链接条目使用.desktop文件扩展名。
    * `Directory`目录类型定义了应用程序菜单中子菜单的外观。目录条目使用.directory文件扩展名。
* `.desktop`文件常常位于`/usr/share/applications/`或者`/usr/local/share/applications/`，用户自定义的位于`~/.local/share/applications/`
* `update-desktop-database ~/.local/share/applications/`可以使得定义在`~/.local/share/applications/`目录下的`.desktop`文件生效。

## XDG图标主题规范
### 格式支持情况
| 扩展名 | 名称及/或描述 | 图形类型 | 容器格式 | 是否支持 |
|-------|-------------|---------|--------|---------|
| [.png](https://en.wikipedia.org/wiki/Portable_Network_Graphics) | 便携式网络图形 | 光栅图 | 不支持 | 支持 |
| [.svg(z)](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) | 可缩放矢量图形 | 矢量图 | 不支持 | 支持(可选) |
| [.xpm](https://en.wikipedia.org/wiki/X_PixMap) | X像素映射 | 光栅图 | 不支持 | 支持(已弃用) |
| [.gif](https://en.wikipedia.org/wiki/Graphics_Interchange_Format) | 图形交换格式 | 光栅图 | 不支持 | 不支持 |
| [.ico](https://en.wikipedia.org/wiki/ICO_(icon_image_file_format)) | MS Windows图标格式 | 光栅图 | 支持 | 不支持 |
| [.icns](https://en.wikipedia.org/wiki/Apple_Icon_Image) | Apple图标图像 | 光栅图 | 支持 | 不支持 |
### 图标目录
1. $HOME/.icons (for backwards compatibility)
2. $XDG_DATA_DIRS/icons
3. /usr/share/pixmaps 
### 图标转换
* 可以使用`imagemagick`包中的`Convert`命令来转换格式：`Convert a.gif a.png`

## XDG默认应用程序管理
XDG默认应用程序的管理主要通过xdg-utils包来提供管理
* xdg-desktop-menu(1) - 安装桌面菜单项
* xdg-desktop-icon(1) - 将桌面条目复制到用户的桌面
* xdg-email(1) - 在用户首选的电子邮件客户端中撰写新电子邮件，可能会填写主题和其他信息
* xdg-icon-resource(1) - 安装图标资源
* xdg-mime(1) - 查询和安装 MIME 类型和关联
* xdg-open(1) - 在用户首选应用程序中打开文件或 URI
* xdg-screensaver(1) - 启用、禁用或暂停屏幕保护程序
* xdg-settings(1) - 获取或设置默认的 Web 浏览器和 URL 处理程序
### xdg-mime使用
* 查询某文件对应的`mime type`
```
xdg-mime query filetype a.b
```
* 查询某`mime type`的默认应用程序
```
xdg-mime query default mime_type
```
* 设置默认程序 
```
xdg-mime default ***.desktop mime_type
```
* 测试某`mime type`的默认程序
```
env XDG_UTILS_DEBUG_LEVEL=10  xdg-mime query default text/html
```
### xdg-mime配置文件位置
1. XDG_CONFIG_HOME/mimeapps.list
2. XDG_DATA_HOME/applications/mimeapps.list or defaults.list or mimeinfo.cache
3. XDG-CONFIG_DIRS/mimeapps.list
4. XDG_DATA_DIRS/applications/mimeapps.list or defaults.list or mimeinfo.cache
[^1]:https://www.freedesktop.org/wiki/Specifications/

