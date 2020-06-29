## 链接

- createConnection

```js
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'me',
  password : 'secret',
  database : 'my_db'
});
connection.connect(function(err) {
  if (err) {
    console.error('error connecting: ' + err.stack);
    return;
  }
  console.log('connected as id ' + connection.threadId);
});
connection.query('SELECT 1 + 1 AS solution', function(err, rows, fields) {
  if (err) throw err;
  console.log('The solution is: ', rows[0].solution);
});
connection.end();
```

    1. 对于一个连接，你所调用的每个方法都是按顺序排队并依次执行的。
    2. 使用end()关闭连接，以确保给mysql服务器发送退出（quit）包以前执行所有剩余的查询。

- 连接池

```js
var mysql = require('mysql');
var pool  = mysql.createPool({
  host     : 'example.org',
  user     : 'bob',
  password : 'secret',
  database : 'my_db'
});
pool.getConnection(function(err, connection) {
  // connected! (unless `err` is set)
   connection.query( 'SELECT something FROM sometable', function(err, rows) {
    // 使用连接执行查询
    connection.release();
    //连接不再使用，返回到连接池
  });
});
```

## 查询

- 第一种
  .query()形式是 .query(sqlString, callback)，第一个参数是一条 SQL 字符串，第二个参数是回调

```js
connection.query('SELECT * FROM `books` WHERE `author` = "David"', function (error, results, fields) {
  // error will be an Error if one occurred during the query
  // results will contain the results of the query
  // fields will contain information about the returned results fields (if any)
});
```

- 第二种
  .query()形式是 .query(sqlString, values, callback)，带有值的占位符 (查看转义查询值)

```js
connection.query('SELECT * FROM `books` WHERE `author` = ?', ['David'], function (error, results, fields) {
  // error will be an Error if one occurred during the query
  // results will contain the results of the query
  // fields will contain information about the returned results fields (if any)
});
```

- 第三种 .query()形式是 .query(options, callback)，在查询时带有大量的高级可选项

```js
connection.query({
  sql: 'SELECT * FROM `books` WHERE `author` = ?',
  timeout: 40000, // 40s
  values: ['David']
}, function (error, results, fields) {
  // error will be an Error if one occurred during the query
  // results will contain the results of the query
  // fields will contain information about the returned results fields (if any)
});
```

### 查询值转义

为了防止[SQL 注入](https://zh.wikipedia.org/wiki/SQL%E6%B3%A8%E5%85%A5)，每当需要在 SQL 查询中使用用户数据时，你都应当提前对这些值进行转义。转义可以通过 mysql.escape(), connection.escape() 或 pool.escape() 方法实现

```js
var userId = 'some user provided value';
var sql    = 'SELECT * FROM users WHERE id = ' + connection.escape(userId);
connection.query(sql, function(err, results) {
  // ...
});
```

另外，也可以使用 ? 作为查询字符串中的占位符，替代你想要转义的值。例如：

```js
connection.query('SELECT * FROM users WHERE id = ?', [userId], function(err, results) {
  // ...
});
```

使用多个占位符则传入的值会依次填入。例如，下方查询中 foo 将等于 a、bar 将等于 b、baz 将等于 c，而 id 将会被赋值为 userId 的值：

```js
connection.query('UPDATE users SET foo = ?, bar = ?, baz = ? WHERE id = ?', ['a', 'b', 'c', userId], function(err, results) {
  // ...
});
```

简洁写法

```js
var post  = {id: 1, title: 'Hello MySQL'};
var query = connection.query('INSERT INTO posts SET ?', post, function(err, result) {
  // Neat!
});
console.log(query.sql); // INSERT INTO posts SET `id` = 1, `title` = 'Hello MySQL'
```

### 查询标识符转义

## 参考文献

- [开源中国](https://www.oschina.net/translate/node-mysql-tutorial)
- [渗透攻防 Web 篇-SQL 注入攻击初级](https://paper.seebug.org/15/)
