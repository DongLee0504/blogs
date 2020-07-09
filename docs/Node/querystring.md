# querystring.parse()

- querystring.parse()方法用于将一个查询字符串解析为 JavaScript 对象。

```js
const querystring = require("querystring");
const str = "foo=bar&abc=xyz&abc=123";
console.log("parse:", querystring.parse(str));
```

输出

> { foo: 'bar', abc: [ 'xyz', '123' ] }

- parse 方法一共接受四个参数  
   `querystring.parse(str[, sep[, eq[, options]]])`
  > str：需要解析的查询字符串  
  > sep：多个键值对之间的分隔符，默认为&  
  > eq：键名与键值之间的分隔符，默认为=  
  > options：配置对象，它有两个属性，decodeURIComponent 属性是一个函数，用来将编码后的字符串还原，默认是 querystring.unescape()，maxKeys 属性指定最多解析多少个属性，默认是 1000，如果设为 0 就表示不限制属性的最大数量。  

<big>*因此可以解析一般的字符串*</big>

```js
const querystring = require("querystring");
const str = "name=xiaoli;age=18;height=185";
console.log("parse2:", querystring.parse(str, ";"));
```

输出

> { name: 'xiaoli', age: '18', height: '185' }
