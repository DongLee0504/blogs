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

# newListener 事件

事件监听被添加前会触发自身的 'newListener' 事件

```js
const EventEmitter = require("events");
const myEvents = new EventEmitter();
// 在添加监听之前触发
myEvents.once("newListener", function (event, listener) {
  console.log("event:", event);
});
// 一个简单的 EventEmitter 实例，绑定了一个监听器。 eventEmitter.on() 用于注册监听器， eventEmitter.emit() 用于触发事件。
myEvents.on("myEvents", function (a, b) {
  console.log("触发事件", a, b);
});
myEvents.emit("myEvents", 1, 2);
```

输出

> event: myEvents  
> 触发事件 1 2
