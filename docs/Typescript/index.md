# 快速上手

- 安装  
  `npm install -g typescript`
- 初始化项目  
  `npm init -y`
- 添加 `tsconfig.json`  
  `tsc --init`

# 基础

## 基础类型

### 未声明类型的变量

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

### 类型推论

> 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

```ts
let myNum = 7;
myNum = "seven";
```

编译过程报错

> error TS2322: Type '"seven"' is not assignable to type 'number'.

### 接口

> 对类的形状（属性）和 行动（方法）进行描述

- 简单例子

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
- 可选属性 & 任意属性
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
- 只读属性
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
  ### 数组
  - 『类型+方括号』表示法
  - 数组泛型（Array<elemType>）
  - 接口表示法
  ```ts
  interface NumberArray {
    [index: number]: number;
  }
  let nums: NumberArray = [1, 2, 3];
  ```
  ### 函数类型
  - 函数表达式
  ```ts
  let myFun: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
  };
  ```
  **在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。**
  - 接口定义函数
  ```ts
  interface SearchFun {
    (source: string, subString: string): boolean;
  }
  let myFunc: SearchFun = function (source: string, subString: string) {
    return source.search(subString) != -1;
  };
  ```
  - 可选参数 & 参数默认值
    **可选参数必须在必选参数后面**
  - 剩余参数
    **剩余参数必须放最后面**
  - 重载
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
