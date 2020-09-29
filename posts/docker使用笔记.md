设置镜像

设置 daemon.json 文件中的字段
{"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]}
可选镜像：https://registry.docker-cn.com、http://hub-mirror.c.163.com、https://docker.mirrors.ustc.edu.cn、https://reg-mirror.qiniu.com
重启服务

> 设置所有容器的 DNS 信息，{"dns" : ["114.114.114.114", "8.8.8.8"]}

常用指令汇总，以下指令前置为 docker

主机环境

version 查看 docker 是否正在运行

- 如果在 server 部分包含错误码，表示 deamon 没有运行，或者当前登录用户没有权限。

镜像

pull [option] NAME[:tag|digest] 下载镜像
images 显示所有镜像
rmi -f IMAGE 强制删除镜像
search NAME 搜索

容器
容器是镜像的运行时的实例，共享其所在主机的操作系统/内核。

run -itd --name [customName] [image] /bin/bash 启动一个容器并让容器内部运行 /bin/bash，（确保容器内有进程在运行，否则将终止）

- -d 让容器在后台运行，不会（-d）进入容器
- -P 将容器内部使用的端口随机映射到我们当前主机上

attach [imageID] 进入容器，但退出后，容器会停止
exec -it 243c32535da7 /bin/bash 进入容器，退出后，容器不会停止
container exec -it 243c32535da7 bash 进入容器，并运行 bash
container ls 查看正在运行的容器列表，-a 显示全部。
ps -a 查看所有的容器
ps -l 查看最后一个创建的容器
port [cid] 查看容器的端口映射
logs -f [cid|cName] 查看容器内部的标准输出
top 查看容器内部运行的进程
inspect [cid|cName] 查看底层信息，返回的是一个 JSON 格式。
start [image] 启动一个已停止的容器
stop [imageID] 停止一个容器
restart [imageID] 重启一个容器
rm [containerID] 删除容器，必须先停止容器，不然报错。参数 -f 为强制删除。
