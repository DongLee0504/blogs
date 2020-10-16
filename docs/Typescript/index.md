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
- 可选属性
  
