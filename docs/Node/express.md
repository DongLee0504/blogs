# 静态文件

- 访问诸如 `Img` `、Css' 、'JavaScript` 文件之类的静态文件，使用 `express.static` 内置中间件函数
- 例如：通过如下代码就可以将 public 目录下的图片、CSS 文件、JavaScript 文件对外开放访问了：
  ```js
  app.use(express.static("public"));
  ```
  现在可以访问 `public` 目录的静态资源了
  ```
  http://localhost:3000/images/kitten.jpg
  http://localhost:3000/css/style.css
  http://localhost:3000/js/app.js
  http://localhost:3000/images/bg.png
  http://localhost:3000/hello.html
  ```
  **Express 在静态目录查找文件，因此，存放静态文件的目录名不会出现在 URL 中。**
  **默认情况是打开静态目录中的 `index.html`**
- 如果想在 URL 中带上目录名，采用如下写法
  ```js
  app.use("/public", express.static("public"));
  ```
  现在，可以访问带目录名的静态资源了
  ```
  http://localhost:3000/public/images/kitten.jpg
  http://localhost:3000/public/css/style.css
  http://localhost:3000/public/js/app.js
  http://localhost:3000/public/images/bg.png
  http://localhost:3000/public/hello.html
  ```
- 以绝对路径访问
  ```js
  app.use("/static", express.static(path.join(__dirname, "public")));
  ```

# 中间件
- 中间件（middleware）就是处理HTTP请求的函数。它最大的特点就是，一个中间件处理完，再传递给下一个中间件。
- 每个中间件可以从App实例，接收三个参数，依次为request对象（代表HTTP请求）、response对象（代表HTTP回应），next回调函数（代表下一个中间件）。每个中间件都可以对HTTP请求（request对象）进行加工，并且决定是否调用next方法，将request对象再传给下一个中间件。
- `next()` 表示将 `request` 对象传递给下一个中间件； `next('出错了')` 带有参数，则代表抛出错误

# request
## 属性
- req.body
  * 必须是 post 请求，依赖的中间件必须有 `bodyParser（ express.json() or express.urlencoded()）`
- req.query
  带?号的参数
- req.params
  带冒号的参数
  
