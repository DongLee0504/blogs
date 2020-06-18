# clean code (javascript)

[toc]

## 1._强类型_

建议使用 === 而不是 == 来判断是否相等

```js
// 如果没有妥善处理的话，可能会出现和预想不一样的结果
0 == false; // true
0 === false; // false
2 == "2"; // true
2 === "2"; // false

const value = "500";
if (value === 500) {
    // 不会执行
    console.log(value);
}

if (value === "500") {
    // 会执行
    console.log(value);
}
```

## 2._变量命名_

坏

```js
let daysSLV = 10;
let y = new Date().getFullYear();

let ok;
if (user.age > 30) {
    ok = true;
}
```

好

```js
const MAX_AGE = 30;
let daysSinceLastVisit = 10;
let currentYear = new Date().getFullYear();

...

const isUserOlderThanAllowed = user.age > MAX_AGE;

```

不要使用多余的无意义的单词来组合命名

坏

```js
let nameValue;
let theProduct;
```

好

```js
let name;
let product;
```

**不要使用无意义的字符/单词来命名，增加额外的记忆负担**

坏

```js
const users = ["John", "Marco", "Peter"];
users.forEach(u => {
    doSomething();
    doSomethingElse();
    // ...
    // ...
    // ...
    // ...
    // 这里u到底指代什么？
    register(u);
});
```

好

```js
const users = ["John", "Marco", "Peter"];
users.forEach(user => {
    doSomething();
    doSomethingElse();
    // ...
    // ...
    // ...
    // ...
    register(user);
});
```

**在某些环境下，不用添加冗余的单词来组合命名。比如一个对象叫 user,那么其中的一个名字的属性直接用 name，不需要再使用 username 了。**

坏

```js
const user = {
  userName: "John",
  userSurname: "Doe",
  userAge: "28"
};
```

好

```js
const user = {
  name: "John",
  surname: "Doe",
  age: "28"
};
```

## 3._函数_

**请使用完整的声明式的名字来给函数命名。比如一个函数实现了某个行为，那么函数名可以是一个动词或则一个动词加上其行为的被作用者。名字就应该表达出函数要表达的行为。**
坏

```js
function notif(user) {
    // implementation
}
```

好

```js
function notifyUser(emailAddress) {
    // implementation
}
```

**避免使用过多参数。最好一个函数只有 2 个甚至更少的参数。参数越少，越容易做测试。**
坏

```js
function getUsers(fields, fromDate, toDate) {
    // implementation
}
```

好

```js
function getUsers({ fields, fromDate, toDate }) {
    // implementation
}

getUsers({
    fields: ["name", "surname", "email"],
    fromDate: "2019-01-01",
    toDate: "2019-01-18"
});
```

**为函数参数设置默认值，而不是在代码中通过条件判断来赋值。**
坏

```js
function createShape(type) {
    const shapeType = type || "cube";
    // ...
}
```

好

```js
function createShape(type = "cube") {
    // ...
}
```

**使用 Objecg.assign 来设置默认对象值。**
坏

```js
const shapeConfig = {
    type: "cube",
    width: 200,
    height: null
};

function createShape(config) {
    config.type = config.type || "cube";
    config.width = config.width || 250;
    config.height = config.width || 250;
}

createShape(shapeConfig);
```

好

```js
const shapeConfig = {
  type: "cube",
  width: 200
  // Exclude the 'height' key
};

function createShape(config) {
  config = Object.assign(
    {
      type: "cube",
      width: 250,
      height: 250
    },
    config
  );

  ...
}

createShape(shapeConfig);
```

**不要使用 true/false 的标签(flag)，因为它实际上让函数做了超出它本身的事情。**
坏

```js
function createFile(name, isPublic) {
    if (isPublic) {
        fs.create(`./public/${name}`);
    } else {
        fs.create(name);
    }
}
```

好

```js
function createFile(name) {
    fs.create(name);
}

function createPublicFile(name) {
    createFile(`./public/${name}`);
}
```

**不要污染全局。如果你需要对现有的对象进行扩展，不要在对象的原型链上定义函数。请使用 ES 的类和继承。**
坏

```js
Array.prototype.myFunc = function myFunc() {
    // implementation
};
```

好

```js
class SuperArray extends Array {
    myFunc() {
        // implementation
    }
}
```

## 4._对象_

- 使用 Object.assign 设置默认属性

```js
// Good:
const menuConfig = {
  title: 'Order',
  // 不包含 body
  buttonText: 'Send',
  cancellable: true
};
function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);
  // config : {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}
createMenu(menuConfig);
```

## 5._尽量不要写全局方法_

- 在 JavaScript 中，永远不要污染全局，会在生产环境中产生难以预料的 bug。举个例子，比如你在 Array.prototype 上新增一个 diff 方法来判断两个数组的不同。而你同事也打算做类似的事情，不过他的 diff 方法是用来判断两个数组首位元素的不同。很明显你们方法会产生冲突，遇到这类问题我们可以用 ES2015/ES6 的语法来对 Array 进行扩展。

```js
// Bad:
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
// Good:
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

## 6. _使用 promise 或者 Async/Await 代替回调_

```js
// Bad:
get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response) => {
  if (requestErr) {
    console.error(requestErr);
  } else {
    writeFile('article.html', response.body, (writeErr) => {
      if (writeErr) {
        console.error(writeErr);
      } else {
        console.log('File written');
      }
    });
  }
});
// Good:
get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

// perfect:
async function getCleanCodeArticle() {
  try {
    const response = await get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await writeFile('article.html', response);
    console.log('File written');
  } catch(err) {
    console.error(err);
  }
}
```

## 7._ES 类_

坏

```js
const Person = function(name) {
    if (!(this instanceof Person)) {
        throw new Error("Instantiate Person with `new` keyword");
    }

    this.name = name;
};

Person.prototype.sayHello = function sayHello() {
    /**/
};

const Student = function(name, school) {
    if (!(this instanceof Student)) {
        throw new Error("Instantiate Student with `new` keyword");
    }

    Person.call(this, name);
    this.school = school;
};

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;
Student.prototype.printSchoolName = function printSchoolName() {
    /**/
};
```

好

```js
class Person {
    constructor(name) {
        this.name = name;
    }

    sayHello() {
        /* ... */
    }
}

class Student extends Person {
    constructor(name, school) {
        super(name);
        this.school = school;
    }

    printSchoolName() {
        /* ... */
    }
}
```

**使用函数调用链。像 jQuery，Lodash 都使用这个模式。你只需要在每一个函数的末尾返回 this，之后的代码会更加的简洁。**
坏

```js
class Person {
    constructor(name) {
        this.name = name;
    }

    setSurname(surname) {
        this.surname = surname;
    }

    setAge(age) {
        this.age = age;
    }

    save() {
        console.log(this.name, this.surname, this.age);
    }
}

const person = new Person("John");
person.setSurname("Doe");
person.setAge(29);
person.save();
```

好

```js
class Person {
    constructor(name) {
        this.name = name;
    }

    setSurname(surname) {
        this.surname = surname;
        // Return this for chaining
        return this;
    }

    setAge(age) {
        this.age = age;
        // Return this for chaining
        return this;
    }

    save() {
        console.log(this.name, this.surname, this.age);
        // Return this for chaining
        return this;
    }
}

const person = new Person("John")
    .setSurname("Doe")
    .setAge(29)
    .save();
```

## 总结

1. 命名要简洁易懂
2. 函数参数多采用结构赋值，参数默认值；函数思想为单一职责
3. 类采用 ES6 语法糖

作者：Fundebug
链接：https://juejin.im/post/5cff04cbf265da1bd3054f31
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
