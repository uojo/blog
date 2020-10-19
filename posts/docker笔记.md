## 学习入门

镜像设置

设置 daemon.json 文件中的字段
{"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]}
可选镜像：https://registry.docker-cn.com、http://hub-mirror.c.163.com、https://docker.mirrors.ustc.edu.cn、https://reg-mirror.qiniu.com
重启服务

> 设置所有容器的 DNS 信息，{"dns" : ["114.114.114.114", "8.8.8.8"]}

### 常用指令

常用指令汇总，以下指令前置为 docker

> 参考[命令大全](https://www.runoob.com/docker/docker-command-manual.html)

#### 主机环境

version 查看 docker 是否正在运行

- 如果在 server 部分包含错误码，表示 deamon 没有运行，或者当前登录用户没有权限。

#### 镜像

- pull [option] [NAME[:tag|digest]] 下载镜像
- images 显示所有镜像
- rmi -f [IMAGE] 强制删除镜像
- search [NAME] 搜索

#### 容器

容器是镜像的运行时的实例（通过基于已有镜像生成一个容器），容器共享所在主机的操作系统/内核。

- `run --name [生成的容器名称] [镜像] [command] [arg...]` 启动一个容器

  - -d 让容器在后台运行，不会（-d）进入容器
  - `-p 主机端口:容器端口` 设定端口映射。将容器内部端口映射到当前主机端口上。
    > 注意：以前端 node 服务为例，如果设定了服务的 host 为 127.0.0.1 或者 192.168 开头，会导致宿主无法访问容器内端口。
  - -P 随机端口映射。将容器内部端口随机映射到主机端口。
  - -i 以交互模式运行容器
  - -t 为容器重新创建一个伪输入终端

> 要确保容器内具有 `/bin/bash` ，否则无法调起。可以试试 `/bin/sh`。

- attach [imageID] 进入容器，但退出后，容器会停止
- exec
  - `exec [container] ps -ef` 执行容器内命令
  - `exec -it 243c32535da7 /bin/bash` 进入容器，退出后，容器不会停止
  - `container exec -it 243c32535da7 bash` 进入容器，并运行 bash
- container ls 查看正在运行的容器列表，-a 显示全部
- ps [options] 查看容器
  - ps -a 查看所有容器
  - ps -l 查看最后一个创建的容器
- top [container] 查看容器内部运行的进程（确保容器内有进程在运行，否则将终止）
  > 显示 PID 是容器内进程在宿主内的 PID
- port [container] 查看容器的端口映射
- start/stop/restart [container] 启动、停止、重启一个或多个容器
- rm [container] 删除容器，必须先停止容器，不然报错。参数 -f 为强制删除
- inspect [container] 查看底层信息，返回的是一个 JSON 格式
- logs -f [container] 查看容器内部的标准输出

### dockerfile 语法

[参考](https://www.runoob.com/docker/docker-dockerfile.html)

build -f [配置文件路径] -t [生成的镜像名称:标签] [配置文件内的上下文路径]

## 实战案例

### 前端 node 项目（服务）生产环境部署

#### 部署过程

##### 1 构建项目镜像。将项目代码、项目依赖服务、脚本与配置等都集中在镜像中。

##### 1.1 使用 dockerfile 构建镜像

##### 2 使用项目镜像创建容器，并启动容器。

##### 2.1 借助 GitLab CI ，实现代码提交后自动部署。
