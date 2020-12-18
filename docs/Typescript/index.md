# 快速上手

- 安装  
  `npm install -g typescript`
- 初始化项目  
  `npm init -y`
- 添加 `tsconfig.json`  
  `tsc --init`

# 基础类型

## 未声明类型的变量

> 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：

```ts
let something;
something = 7;
something = "seven";
```

等价于

```ts
let something: any;
something = 7;
something = "seven";
```

## 类型推论

> 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

```ts
let myNum = 7;
myNum = "seven";
```

编译过程报错

> error TS2322: Type '"seven"' is not assignable to type 'number'.

## 接口

> 对类的形状（属性）和 行动（方法）进行描述

### 简单例子

```ts
interface Person {
  name: string;
  age: number;
}
const tom: Person = {
  name: "Tom",
  age: 18,
};
```

编译

```js
"use strict";
var tom = {
  name: "Tom",
  age: 18,
};
```

- **变量比接口多或少属性都不行的**

### 可选属性 & 任意属性

```ts
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}
const tom: Person = {
  name: "Tom",
  age: 18, // 可选属性
  sex: "男", // 任意属性
};
```

### 只读属性

```ts
interface Person {
  readonly id: number; // 只读属性
  name: string;
  age?: number;
  [propName: string]: any;
}
const tom: Person = {
  id: 1,
  name: "Tom",
  age: 18,
  sex: "男",
};
```

**注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**

## 数组

### 『类型+方括号』表示法

### 数组泛型（Array<elemType>）

### 接口表示法

```ts
interface NumberArray {
  [index: number]: number;
}
let nums: NumberArray = [1, 2, 3];
```

## 函数类型

### 函数表达式

```ts
let myFun: (x: number, y: number) => number = function (x: number, y: number): number {
  return x + y;
};
```

**在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。**

### 接口定义函数

```ts
interface SearchFun {
  (source: string, subString: string): boolean;
}
let myFunc: SearchFun = function (source: string, subString: string) {
  return source.search(subString) != -1;
};
```

### 可选参数 & 参数默认值

**可选参数必须在必选参数后面**

### 剩余参数

**剩余参数必须放最后面**

### 重载

```ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: string | number): string | number {
  if (typeof x === "number") {
    return Number(x.toString().split("").reverse().join(""));
  } else {
    // 注意这里
    return x.split("").reverse().join("");
  }
}
```

## 类型断言

类型断言（Type Assertion）可以用来手动指定一个值的类型。

### 语法

`值 as 类型` 或 `<类型>值`

### 用途

- 将一个联合类型断言为其中一个类型
  当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型中共有的属性或方法：

  ```ts
  interface Cat {
    name: string;
    run(): void;
  }
  interface Finish {
    name: string;
    swim(): void;
  }
  function getName(animal: Cat | Finish) {
    return animal.name;
  }
  const animal: Cat = {
    name: "小胖",
    run() {},
  };
  ```

  上面代码因为访问了共同的属性，所以不会报错  
   下面的代码就会报错

  ```ts
  interface Cat {
    name: string;
    run(): void;
  }
  interface Finish {
    name: string;
    swim(): void;
  }
  function getName(animal: Cat | Finish) {
    return animal.run();
  }
  const animal: Cat = {
    name: "小胖",
    run() {},
  };
  ```

  > Property 'run' does not exist on type 'Cat | Finish'.
  > Property 'run' does not exist on type 'Finish'.
  > 下面程序使用了类型断言，可以正常运行

```ts
interface Cat {
  name: string;
  run(): void;
}
interface Finish {
  name: string;
  swim(): void;
}
function getName(animal: Cat | Finish) {
  (animal as Cat).run(); // 将 animal 断言为 Cat
}
const animal: Cat = {
  name: "小胖",
  run() {
    console.log("i can run");
  },
};
getName(animal);
```

# 进阶

## 类型别名

看下面代码

```ts
const ladies: { name: string; age: number }[] = [
  { name: "wang", age: 18 },
  { name: "liu", age: 18 },
];
```

定义数组的对象时比较麻烦

```ts
type Lady = { name: string; age: number };
const ladies: Lady[] = [
  { name: "wang", age: 18 },
  { name: "liu", age: 18 },
];
```

可以使用`type`定义一个类型

## 字符串字面量类型

```ts
type EventNames = "click" | "scroll" | "mousemove";
function handleEvent(ele: Element, event: EventNames) {
  // do something
}

handleEvent(document.getElementById("hello"), "scroll"); // 没问题
handleEvent(document.getElementById("world"), "dblclick"); // 报错，event 不能为 'dblclick'
```

## 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。

```ts
enum Days {
  Sun,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}
```

编译为

```js
"use strict";
var Days;
(function (Days) {
  Days[(Days["Sun"] = 0)] = "Sun";
  Days[(Days["Mon"] = 1)] = "Mon";
  Days[(Days["Tue"] = 2)] = "Tue";
  Days[(Days["Wed"] = 3)] = "Wed";
  Days[(Days["Thu"] = 4)] = "Thu";
  Days[(Days["Fri"] = 5)] = "Fri";
  Days[(Days["Sat"] = 6)] = "Sat";
})(Days || (Days = {}));
```

`Days` 对象为

```json
{
  0: "Sun"
  1: "Mon"
  2: "Tue"
  3: "Wed"
  4: "Thu"
  5: "Fri"
  6: "Sat"
  Fri: 5
  Mon: 1
  Sat: 6
  Sun: 0
  Thu: 4
  Tue: 2
  Wed: 3
}
```

### 手动赋值

```ts
enum Days {
  Sun = 7,
  Mon = 1,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}
```

编译为

```js
"use strict";
var Days;
(function (Days) {
  Days[(Days["Sun"] = 7)] = "Sun";
  Days[(Days["Mon"] = 1)] = "Mon";
  Days[(Days["Tue"] = 2)] = "Tue";
  Days[(Days["Wed"] = 3)] = "Wed";
  Days[(Days["Thu"] = 4)] = "Thu";
  Days[(Days["Fri"] = 5)] = "Fri";
  Days[(Days["Sat"] = 6)] = "Sat";
})(Days || (Days = {}));
```

上面的例子中，未手动赋值的枚举项会接着上一个枚举项递增。

## 类

`ES5` 采用构造函数定义类  
`ES6` 、 `TS` 采用 `class` 定义类


