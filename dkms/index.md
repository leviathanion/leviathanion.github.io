# DKMS

# DKMS
* DKMS动态内核模块支持（DKMS）是用来生成 Linux 的内核模块的一个框架
    * 虽然可以通过Readme文件或者INSTALL文件来编译新的驱动或者某些模块，但当内核版本变动时，需要重新编译这些模块，较为复杂
    * 因此DKMS一般适用于还未集成到内核中的模块，或者魔改版Linux内核
    * 同时当新的内核安装时，DKMS 支持的内核模块会自动重建。
* 前置条件
    * 安装DKMS
    * 安装Linux内核的头文件(标准Linux版本头文件为linux-headers。非标准版需要自己寻找)
![DKMS工作流程](/DKMS/DKMS工作流程.jpg "DKMS工作流程")
* `dkms status`列出当前模块的状态和版本，包括内核中集成的模块
* `dkms add a/1.0`添加模块
* `dkms build a/1.0`构建模块
* `dkms install a/1.0`安装nvidia模块的334.21版本
* `dkms uninstall a/1.0`卸载该模块
* `dkms remove a/1.0 --all`完全移除一个模块
* `dkms autoinstall`重新构建所有模块


