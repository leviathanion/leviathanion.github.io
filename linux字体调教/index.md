# Linux字体调教

# Linux字体调教
> 本文参考自[^2][^3]
## 具体字体配置
* 使用pacman安装字体,或将其放到指定目录中
* 执行`fc-cache -fv`更新字体缓存
* 对于不支持fontconfig的旧程序
    * 在字体目录创建索引`mkfontscale` `mkfontdir`
    * 执行`xset +fp $dir`添加字体目录 
    * `xset fp rehash`强制刷新
    * `xlsfonts | grep fontname`查看是否生效
    * 或者在`/etc/X11/xorg.conf`添加以下代码
    ```shell
    # 让X.org知道自定义字体目录 
    Section "Files"
     FontPath    "/usr/share/fonts/100dpi"
     FontPath    "/usr/share/fonts/75dpi"
     FontPath    "/usr/share/fonts/TTF"
     FontPath    "/usr/share/fonts/util"
    EndSection
    ```
## 前瞻知识
* 一个字体文件，可以提供多个字体族名 (family)。
> 比如 Arch Linux 用户在安装 wqy-microhei 后
> * 系统端增加了 wqy-microhei.ttc 这个字体文件，分别提供「WenQuanYi Micro Hei」「文泉驛微米黑」,「文泉驿微米黑」三个字体族名。
> * 我们可以运行 fontconfig 提供的命令行工具 fc-list 去查看系统上已安装的字体以及它们对应的字体族名。
* **sans-serif，serif，monospace**，则是三个通用字体族名 (generic family)
> * 它们不是真实存在的字体，而是分别指示程序去使用**无衬线、衬线、等宽字体**。
> * 桌面程序需要去查询**fontconfig**来选择字体。
> * 我们可以通过**fontconfig的配置**去精准控制程序使用的字体。
* 桌面程序使用字体的流程
    * 桌面程序根据自身的设置，传递font pattern使用fontconfig来查找字体
    * fontconfig依据自身配置的规则来修改font pattern,返回结果给桌面程序
    * 桌面程序使用系统字体并通过FreeType等字体渲染引擎许绘制文字
> 在这里要明确一点，给 GTK 程序、Qt 程序、虚拟终端的配置文件中设置它们使用的字体，和设置 fontconfig 的配置文件是两码事。虽然在一些桌面环境的全局设置中，设置字体会同时修改 GTK、Qt、fontconfig 的配置。
> 
> 此外，桌面程序可以完全不遵守 fontconfig，或者说，程序可以不使用 fontconfig 来查找字体。当然，这种情况很少，现代桌面程序基本上都会遵守我们的 fontconfig 配置。

## fontconfig配置文件的位置
* fontconfig配置文件主要包含以下位置
    * 系统级 `/etc/fonts/fonts.conf`,`/etc/fonts/conf.d/*.conf`
    * 用户级`$XDG_CONFIG_HOME$/fontconfig/fonts.conf`,`$XDG_CONFIG_HOME$/fontconfig/conf.d/*.conf`
    * Fontconfig会综合所有配置文件到一个配置文件中：`/etc/fonts/fonts.conf`,因此该文件不应被修改
* Fontconfig首先读取`/etc/fonts/fonts.conf`
    * 该文件有如下语句,将`/etc/fonts/conf.d`目录中的所有文件引入读取中
    ```shell
    <include ignore_missing="yes">conf.d</include>
    ```
    * 上述目录的配置文件按照文件前的数字的顺序进行读取，而`50-usr.conf`包含如下语句,读取用户目录下的配置文件，`prefix="xdg"`代表`XDG_CONFIG_HOME`
    ```shell
    <include ignore_missing="yes" prefix="xdg">fontconfig/conf.d</include>
    <include ignore_missing="yes" prefix="xdg">fontconfig/fonts.conf</include>
    <include ignore_missing="yes" deprecated="yes">~/.fonts.conf.d</include>
    <include ignore_missing="yes" deprecated="yes">~/.fonts.conf</include>
    ```
    * Fontconfig在读取完用户配置文件后，接着读完`/etc/fonts/conf.d`中剩余的配置文件
> * `/etc/fonts/conf.avail/`中包含了很多没有默认启用的配置，很多是字体软件包安装后，往里面放入的
> * `pacman -Qqo /etc/fonts/conf.avail`可以查看哪些软件包修改了此目录
> * `/etc/fonts/conf.d`中启用的配置文件很多这个目录下文件的软连接，因此可以通过将此目录下的配置文件软连接到`/etc/fonts/conf.d`来启用配置

## 字体文件的位置
* 字体文件的位置同样在`/etc/fonts/fonts.conf`中定义,如下所示
```xml
<dir>/usr/share/fonts</dir>
<dir>/usr/local/share/fonts</dir>
<dir prefix="xdg">fonts</dir>
<!-- the following element will be removed in the future -->
<dir>~/.fonts</dir>
```
* 可以通过下述语句来查看已安装的字体`pacman -Qqo /usr/share/fonts`
## fontconfig配置文件语法
### 示例一
* 配置文件是XML语句,内容被包含在`<fontconfig>`标签中
* 大量出现的配置语句是`<match>...<test>...<edit>...`
* 下述是语法示例
```xml
<match target="pattern">
  <test name="family">
    <string>monospace</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>Iosevka Custom</string>
    <string>Noto Sans Mono CJK SC</string>
    <string>emoji</string>
  </edit>
</match>
```
> * `taget="pattern"`代表对font pattern进行操作,`target="font"`则是对单个字体进行操作
> * `test`可选的测试条件，当满足条件是，执行`edit`
> * `<edit name="family" mode="prepend" binding="strong">`，执行编辑操作，`name="family"`表明被操作对象是是 font pattern 中的 family
> * `mode="prepend"`表示在monospace前添加三种字体,`binding="strong"`则是强绑定的意思,只需要知道强绑定默认优先级比弱绑定更高
> * 其实，`<match>...<test>...<edit name="family" mode="prepend">...`等价于`<alias>...<family>...<prefer>...`
### 示例二
```xml
<match target="pattern">
  <test name="family">
    <string>Cantarell</string>
  </test>
  <edit name="family" mode="assign" binding="strong">
    <string>Noto Sans</string>
  </edit>
</match>
```
* `mode="assign"`表示将Cantarell修改成Noto Sans
### 示例三
```xml
<match target="pattern">
  <test qual="all" name="family" compare="not_eq">
    <string>sans-serif</string>
  </test>
  <test qual="all" name="family" compare="not_eq">
    <string>serif</string>
  </test>
  <test qual="all" name="family" compare="not_eq">
    <string>monospace</string>
  </test>
  <edit name="family" mode="append_last">
    <string>sans-serif</string>
  </edit>
</match>
```
* test 中的`qual="all" name="family"`表示对 font pattern 中的每一个字体都执行测试
* `compare="not_eq"`就是 family 不等于这个值的时候。
* 当三条测试规则都满足的时候，也就是 font pattern 没有任何通用字体族名的时候，默认 fallback 到 sans-serif 无衬线字体
* edit 操作里面的`mode="append_last"`表示 sans-serif 添加在 family 列表的最末尾。
* 如果在 test 中包含多条<string>的时候，fontconfig 会报错。

## fontconfig配置来实现字体调教
### 中英文混搭问题
* 异体字问题
    * Noto Sans CJK 中的异体字，是在 相同的 Unicode 码位下，不同的语言会使用不同的字形。
    * 因此不正确的配置会导致中文变成繁体字，日文等
* 中文字体的西文部分
    * 中文字体携带的英文字符有可能十分糟糕
    * 需要进行优先级设定或者替换
* 全角引号
    * 英文中的单引号有两种，'(U+0027) 和’(U+2019)。可能会觉得前一种出现在英文文本中，后一种出现在中文文本中，并且宽度和中文等宽。
    * 然而，英语世界中的一些人在英文文章中也是会使用后一种的。所以，字体在显示后一种引号的时候，究竟是和英文字母一样窄，还是该和中文字体等宽呢？如果在英文文章中显示成全宽字符则会显得突兀。
### 配置示例
* 基本思路为设置字体为通用字体族sans,serif,sans-serif,monospace
* 在`~/.config/fontconfig/fonts.conf`编辑字体配置，示例如下
```xml
<!-- Default system-ui fonts -->
<match target="pattern">
  <test name="family">
    <string>system-ui</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>sans-serif</string>
  </edit>
</match>

<!-- Default sans-serif fonts-->
<match target="pattern">
  <test name="family">
    <string>sans-serif</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>Noto Sans</string>
    <string>Noto Sans CJK SC</string>
    <string>Noto Color Emoji</string>
  </edit>
</match>

<!-- Default serif fonts-->
<match target="pattern">
  <test name="family">
    <string>serif</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>Noto Serif</string>
    <string>Noto Serif CJK SC</string>
    <string>Noto Color Emoji</string>
  </edit>
</match>

<!-- Default monospace fonts-->
<match target="pattern">
  <test name="family">
    <string>monospace</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>JetBrainsMono Nerd Font</string>
    <string>Nerd Font</string>
    <string>Noto Color Emoji</string>
  </edit>
</match>
```
* 异体字修改示例
```xml
<match target="pattern">
  <test name="lang">
    <string>zh-HK</string>
  </test>
  <test name="family">
    <string>Noto Sans CJK SC</string>
  </test>
  <edit name="family" binding="strong">
    <string>Noto Sans CJK HK</string>
  </edit>
</match>
<!-- 另外请再重复 zh-TW、ja、ko -->
```
* 上述配置文件中由于优先使用了英文字体，因此会导致全角符号问题，可以通过如下方式修改(因为影响不大，我并未修改)
    * 调换中文和英文字体的顺序
    * 替换英文字体
    ```xml
    <match target="pattern">
    <test name="lang" compare="contains">
        <string>en</string>
    </test>
    <test name="family" compare="contains">
        <string>Noto Sans CJK</string>
    </test>
    <edit name="family" mode="prepend" binding="strong">
        <string>Noto Sans</string>
    </edit>
    </match>
    ```
## 字体渲染
* 虽然 fontconfig 本身不负责渲染字体，实际渲染是由程序交给 freetype 来完成的；但是 fontconfig 在处理 font pattern 的时候可以修改一些渲染参数，比如是否开启 hinting 和子像素渲染，是否开启连字特性。同时也可以根据实际情况来选择和过滤字体，比如根据像素尺寸分别指定字体，又或者控制何时使用点阵字体。
* 具体可参考arch wiki中的配置[^1]


[^1]:https://wiki.archlinux.org/title/Font_configuration
[^2]:https://catcat.cc/post/2020-10-31/
[^3]:https://catcat.cc/post/2021-03-07/

