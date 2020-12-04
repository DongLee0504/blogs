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
