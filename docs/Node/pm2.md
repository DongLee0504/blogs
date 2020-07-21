[toc]

# pm2

## 启动挂钩

## .保存您的进程列表

```
pm2 save
```

## 开机自启动

```
pm2 startup
```

## 恢复服务（save 过的服务）

```
pm2 resurrect
```

[https://github.com/Unitech/pm2/issues/2775](https://github.com/Unitech/pm2/issues/2775)

## 指定 env

- 配置文件
  ```js
  module.exports = {
    apps: [
      {
        name: "myapp",
        script: "./app.js",
        watch: true,
        env: {
          PORT: 3000,
          NODE_ENV: "development",
        },
        env_production: {
          PORT: 80,
          NODE_ENV: "production",
        },
      },
    ],
  };
  ```
- env 为默认环境，我们可以指定环境 `pm2 start ecosystem.config.js --env production`

# linux 下安装 node

```js
Linux 上安装 Node.js
直接使用已编译好的包
Node 官网已经把 linux 下载版本更改为已编译好的版本了，我们可以直接下载解压后使用：

wget https://nodejs.org/dist/v10.9.0/node-v10.9.0-linux-x64.tar.xz    // 下载
tar xf  node-v10.9.0-linux-x64.tar.xz       // 解压
cd node-v10.9.0-linux-x64/                  // 进入解压目录
./bin/node -v                               // 执行node命令 查看版本
v10.9.0
解压文件的 bin 目录底下包含了 node、npm 等命令，我们可以使用 ln 命令来设置软连接：
sudo ln -s /home/yztapp1/node-v10.9.0-linux-x64/bin/npm  /usr/bin/npm
sudo ln -s /home/yztapp1/node-v10.9.0-linux-x64/bin/node  /usr/bin/node
```

# Supervisor

## 安装

- Windows

  ```
  npm install -g supervisor
  ```

- Linux 或者 mac

  ```
  sudo npm install -g supervisor
  ```

## 启动项目

- 项目根目录下

  ```
  supervisor myapp
  ```
