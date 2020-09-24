# pm2 使用笔记

- pm2 reload <app_name>
- pm2 reload ecosystem.config.js
- pm2 reload ecosystem.config.js --only app

### Startup Hook

- pm2 startup
- pm2 save
- pm2 cleardump
  - If you want to create an empty dump file you should use
- pm2 unstartup

### Load-Balancing (cluster mode)

- pm2 start app.js -i max

### Entrypoint

The entrypoint.js lets you precisely contol your application flow and organize the link between the Node.js runtime and PM2.

- pm2 start entrypoint.js

### Development Tools

- pm2 serve <path> <port>
- pm2 serve
  - 默认当前执行目录，端口 8080

### log

- pm2 logs <app?> #查看日志
- pm2 flush #清空日志
