# Conda使用教程

# Conda使用教程
## 安装
### Windows
* 官网下载`exe`文件，直接安装
### Linux
* 下载`.sh`文件
* 授权`chmod +x Miniconda3-latest-Linux-x86_64.sh`
* 运行`./Miniconda3-latest-Linux-x86_64.sh`
### MacOS
* Miniconda
    * 下载`.sh`文件
    * 运行`./Miniconda3-latest-Linux-x86_64.sh`
* Anaconda
    * 官网下载`pkg`文件，直接安装
## 创建Conda环境
* 创建Python环境：`conda create -n/--name <env_name> -p /path/to/file python=<version> `
* `-p`参数可以省略
## 查看环境
* 查看所有环境：`conda env list `
## 切换环境
* 切换环境：`conda activate <env_name>`
* 退出环境：`conda deactivate`
## 删除环境或依赖
* 删除环境：`conda remove -n/--name <env_name>`
* 删除依赖：`conda remove -n/--name <env_name> <package_name>`
## 导出和导入环境
* 导出环境：`conda list --explicit > /path/to/file`
* 导入环境：`conda install --file /path/to/file`
## 查看已安装依赖
* 查看所有依赖：`conda list`
* 查看单个依赖：`conda list -n <package_name>`
## 搜索依赖
* 搜索依赖：`conda search <package_name>`
## 安装依赖
* （已激活环境）安装依赖：`conda install <package_name>`
* （未激活环境）安装依赖：`conda install -n/--name <env_name> <package_name>`
## 更换镜像源
### 编辑`.condarc`文件
* 阿里源
```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - http://mirrors.aliyun.com/anaconda/pkgs/main
  - http://mirrors.aliyun.com/anaconda/pkgs/r
  - http://mirrors.aliyun.com/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://mirrors.aliyun.com/anaconda/cloud
  msys2: http://mirrors.aliyun.com/anaconda/cloud
  bioconda: http://mirrors.aliyun.com/anaconda/cloud
  menpo: http://mirrors.aliyun.com/anaconda/cloud
  pytorch: http://mirrors.aliyun.com/anaconda/cloud
  simpleitk: http://mirrors.aliyun.com/anaconda/cloud
```
* 清华源
```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```
### 命令行配置
```
#查看当前conda配置
conda config --show channels
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud//pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
 
#设置搜索是显示通道地址
conda config --set show_channel_urls yes
```





