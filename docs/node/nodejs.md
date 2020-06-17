打印两次问题

# 路径

- \_\_dirname： 获得当前执行文件所在目录的完整目录名
- \_\_filename： 获得当前执行文件的带有完整绝对路径的文件名
- process.cwd()：获得当前执行 node 命令时候的文件夹目录名
- ./： 文件所在目录

# cross-env

- 使用 cross-env 设置 npm script 环境变量

```json
"scripts": {
    "estart": "cross-env NODE_ENV=development && electron .",
    "start": "webpack --config webpack.config.js",
    "dev": "cross-env NODE_ENV=development && webpack-dev-server",
    "build": "cross-env NODE_ENV=production && webpack",
    "package": "npm run build && electron-builder build --win --x64"
  },
  // 或者使用set
  "scripts": {
    "start": "set NODE_ENV=dev& supervisor -w app.js,routes,bin,public,views,model ./bin/www",
    "dev": "set NODE_ENV=dev& node build/dev-server.js",
    "build": "node build/build.js"
  },
```

# 打印两次问题

```js
const http = require('http');
const server = http.createServer(function (req, res) {
console.log('服务启动')
res.end()
})
server.listen(8080)
```

- 这段代码启动服务后，在谷歌浏览器输入 localhost:8080 在控制器会打印两次 ‘服务启动’ 这个是谷歌浏览器的原因，类似获取 favicon，在火狐浏览器则是正常的。

# progress.cwd()与 progress.\_\_dirname

- 注意，process.cwd()与**dirname 的区别。前者进程发起时的位置，后者是脚本的位置，两者可能是不一致的。比如，node ./code/program.js，对于 process.cwd()来说，返回的是当前目录（.）；对于**dirname 来说，返回是脚本所在目录，即./code/program.js。

# progress.nextTick()

- process.nextTick 将任务放到当前一轮事件循环（Event Loop）的尾部。

```js
process.nextTick(function () {
  console.log('下一次Event Loop即将开始!');
});
```

上面代码可以用 setTimeout(f,0)改写，效果接近，但是原理不同。

```js
setTimeout(function () {
  console.log('已经到了下一轮Event Loop！');
}, 0)
```

**setTimeout(f,0)是将任务放到下一轮事件循环的头部，因此 nextTick 会比它先执行。另外，nextTick 的效率更高，因为不用检查是否到了指定时间。**

# require

- 模块分为三类

1. 内置模块（‘fs’，‘http’等）
2. 第三方模块（node_modules）
3. 自定义模块

- 查找规则

1. 内置模块和第三方模块 require('模块名')，从当前项目的 node_modules 查找，然后依次进入父目录，查找父目录下的 node_modules 目录；依次迭代，直到根目录下的 node_modules 目录。

# 模板引擎

```js
script(src="a.js")
a(href="http://www.baidu.com") 百度
div(style="width:200px,height:200px")
div(style={width:'200px',height: '200px'})
div(class="a b c")
div(class=['a', 'b', 'c'])
```
