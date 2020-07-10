# event emitter 接口部署

event emitter 接口可以部署到任意对象上，使该对象也可以发布订阅消息

```js
function Dog(name) {
  this.name = name;
}
Dog.prototype.__proto__ = require("events").EventEmitter.prototype;
let duoduo = new Dog("duodou");
duoduo.on("bark", function () {
  console.log(`${this.name} is barking`);
});
setTimeout(() => {
  duoduo.emit("bark");
}, 5000);
```
