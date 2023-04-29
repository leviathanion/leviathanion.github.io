# Docker使用

# Docker使用
## Docker架构
* image(镜像)：可以看作是一个root文件系统，是对环境和软件的包装
* container(容器)：镜像类似于类，容器类似于实例
* repository(仓库)：镜像库
## Docker基本使用
### 启动本地dokcer软件
* sudo systemctl start docker
### 通用
* 查看所占空间`docker system df`
### repository使用
* 获取image`docker pull [选项] [仓库地址[:端口号]/仓库名[:标签]]`
    * 不加仓库地址，默认为docker hub
### image管理
* 列出本机的image文件:`docker image ls`
* 根据容器生成image文件:`docker commit [选项] <容器名ID或容器名> [<仓库名>[:<标签>]]`
* 删除本机的image文件:`docker image rm [imageName]`
### container管理
#### 文件管理
* 列出本机运行的container文件:`docker container ls`
* 列出本机所有的container文件:`docker container ls --all`
* 导出容器:`docker export <containerID> > *.tar`
* 导入容器:`docker import *.tar <镜像名>:<标签>`
* 删除本机的container文件:`docker container rm <containerID>`
* 清理所有处于停止状态的容器:`docker container prune`
#### 运行管理
* 启动容器:`docker run [选项] [imageName] [sh终端]`
    * 选项可以省略
    * `-p 本地端口号：docker端口号`端口映射
    * `-d`后台运行不进入
    * `-i`是允许交互式操作
    * `-t`是终端
* 进入容器:`docker exec -it <容器名/id> [sh终端]`
* 终止容器:`docker container stop <容器名/id>`
* 启动已终止容器:`docker container start <容器名/id>`
* 重启容器:`docker container restart <容器名/id>`
## Podman
* Podman与docker完全兼容，只需要将docker命令里的docker更换为Podman即可



