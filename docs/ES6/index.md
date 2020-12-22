# 变量解构赋值

## 数组解构赋值

### 基本用法

```js
let [a, b, c] = [1, 2, 3];
```

**本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。**

### 默认值

```js
let [foo = true] = [];
foo; // true

let [x, y = "b"] = ["a"]; // x='a', y='b'
let [x, y = "b"] = ["a", undefined]; // x='a', y='b'
```

**注意，ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于 `undefined`，默认值才会生效。**

```js
let [x = 1] = [undefined];
x; // 1

let [x = 1] = [null];
x; // null
```

上面代码中，如果一个数组成员是`null`，默认值就不会生效，因为`null`不严格等于`undefined`。

---

## 对象解构赋值

### 对象的方法解构赋值

```js
// 例一
let { log, sin, cos } = Math;

// 例二
const { log } = console;
log("hello"); // hello
```

### 变量名与属性名不一致

```js
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz; // "aaa"
foo; // VM143:2 Uncaught ReferenceError: foo is not defined
```

**对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。**

---

## 注意点

### 已经申明的变量解构赋值

```js
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error
```

JavaScript 引擎会将{x}理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免 JavaScript 将其解释为代码块，才能解决这个问题。

```js
// 正确的写法
let x;
({ x } = { x: 1 });
```

### 对数组进行对象属性的解构

```js
let arr = [1, 2, 3];
let { 0: first, [arr.length - 1]: last } = arr;
first; // 1
last; // 3
```

---

# 字符串

## 新增方法

- includes()：返回布尔值，表示是否找到了参数字符串。
- startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
- endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
- repeat(): 方法返回一个新字符串，表示将原字符串重复 n 次。
- padStart(): 头部补全
- padEnd(): 尾部补全
- trimStart(): 消除头部空格
- trimEnd(): 消除尾部空格
- replaceAll(regexp|substr, newSubstr|function): 一次性替换所有匹配

---

# class

## 一般例子

```js
"use strict";
class Animal {
  constructor(name) {
    this.name = name;
  }
  run() {
    console.log(`${this.name} can run`);
  }
}
const cat = new Animal("tom");
```

- `class` 类必须有 `constructor` 函数，在实例化时自动执行，如不显示添加，js 引擎会自动添加
- `this` 为实例对象
- > `cat.hasOwnProperty('name') // true`  
  > 实例的属性显示定义在本身（即定义在 this 对象上），实例对象自身的属性
- > `cat.hasOwnProperty('run') // false`  
  >  `cat.__proto__.hasOwnProperty('run') // true`  
  >  类的方法为原型对象的属性

## 属性表达式

```js
"use strict";
const methodName = "getProp";
class Animal {
  constructor(name) {
    this.name = name;
  }
  run() {
    console.log(`${this.name} can run`);
  }
  [methodName]() {
    console.log(5555);
  }
}
```

## class 表达式

```js
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```

> `Me` 只能在内部使用，如果内部没有用到，`Me`可以省略  
> 在外部只能用 `MyClass` 引用

## this

类的方法内部如果含有 this，**_它默认指向类的实例_**。但是，必须非常小心，一旦单独使用该方法，很可能报错。

```js
class Logger {
  printName(name = "there") {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

上面代码中，printName 方法中的 this，默认指向 Logger 类的实例。但是，如果将这个方法提取出来单独使用，this 会指向该方法运行时所在的环境（由于 class 内部是严格模式，所以 this 实际指向的是 undefined），从而导致找不到 print 方法而报错。

### 解决方案

在构造方法中绑定 this (实例化时构造函数自动执行)

```js
class Logger {
  constructor() {
    this.printName = this.printName.bind(this);
  }

  // ...
}
```

## 静态方法

静态方法不被实例继承，类可以直接调用

### 例 1

```js
class Foo {
  static classMethod() {
    return "hello";
  }
}

Foo.classMethod(); // 'hello'

var foo = new Foo();
foo.classMethod();
// Uncaught TypeError: foo.classMethod is not a function
```

### 例 2

**_注意，如果静态方法包含 this 关键字，这个 this 指的是类，而不是实例。_**

```js
class Foo {
  static bar() {
    this.baz();
  }
  static baz() {
    console.log("hello");
  }
  baz() {
    console.log("world");
  }
}

Foo.bar(); // hello
```

### 例 3

父类的静态方法，可以被子类继承。

```js
class Foo {
  static classMethod() {
    return "hello";
  }
}

class Bar extends Foo {}

Bar.classMethod(); // 'hello'
```

## 实例属性的写法

### 构造方法里面

### 类的头部

```js
class Foo {
  bar = "hello";
  baz = "world";

  constructor() {
    console.log(this.bar);
  }
}
const foo = new Foo();
// hello
```

## 类的继承

- 采用关键字 `extends`
- 子类必须在`constructor`方法中调用 `super` 方法，否则新建实例时会报错。
- 如果子类没有写`constructor`方法，`super` 会默认调用

### 例 1

```js
class Point {
  /* ... */
}

class ColorPoint extends Point {
  constructor() {}
}

let cp = new ColorPoint();
// Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```

上例中，子类 `constructor`方法没有调用 `super`，所以报错

### 例 2

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类 constructor 后才可以使用 this
    this.color = color; // 正确
    console.log(this.x);
  }
}
```
