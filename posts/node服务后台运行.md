## 一、利用 forever

forever 是一个 nodejs 守护进程，完全由命令行操控。forever 会监控 nodejs 服务，并在服务挂掉后进行重启。

1、安装 forever

npm install forever -g
2、启动服务

service forever start
3、使用 forever 启动 js 文件

forever start index.js
4、停止 js 文件

forever stop index.js
5、启动 js 文件并输出日志文件

forever start -l forever.log -o out.log -e err.log index.js
6、重启 js 文件

forever restart index.js
7、查看正在运行的进程

forever list

## 二、pm2 是一个进程管理工具,可以用它来管理你的 node 进程,并查看 node 进程的状态,当然也支持性能监控,进程守护,负载均衡等功能

npm install -g pm2
pm2 start app.js // 启动
pm2 start app.js -i max //启动 使用所有 CPU 核心的集群
pm2 stop app.js // 停止
pm2 stop all // 停止所有
pm2 restart app.js // 重启
pm2 restart all // 重启所有
pm2 delete app.js // 关闭

## 三、nodejs 自带 node.js 自带服务 nohup，不需要安装别的包。

缺点：存在无法查询日志等问题,关闭终端后服务也就关闭了， 经测试是这样的。

nohup node \*\*\*.js &
