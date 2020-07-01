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
