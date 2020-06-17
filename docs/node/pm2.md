[toc]

# pm2

## 启动挂钩

- .保存您的进程列表

```
pm2 save
```

- 开机自启动

```
pm2 startup
```

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

项目根目录下

```
supervisor myapp
```

## 更多方法

```
supervisor
```
