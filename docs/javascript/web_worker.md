# Web Worker

<a name="41e87233"></a>
# web work


<a name="a4d3b02a"></a>
## 概述


- JavaScript 是单线程模型，也就是说所有的任务只能在一个线程上完成。前面的任务没有完成，后面的任务只能排队。
- 随着电脑计算能力不断增强，尤其多核 CPU 的出现，单进程带来不便，而且无法充分发挥计算机的计算能力。
- web worker 的作用就是允许 JavaScript 创建 worker 线程。处理计算密集型或高延迟的任务，给主线程减负。



<a name="b4d3c72e"></a>
## 特点


- worker 线程一旦创建成功，就会始终运行，不会被主线程的操作（用户点击，提交表单）打断。



<a name="bc120b21"></a>
## 用法


- 主线程采用 new 命令，调用 Worker 构造函数，新建一个 worker 线程
```javascript
let myWorker = new Worker("./worker.js");
```

- Worker 构造函数的参数是一个脚本文件，该文件就是 worker 线程所要执行的任务
- 主线程向 worker 发消息



```javascript
myWorker.postMessage("hello worker");
```


- worker 线程接受消息



```javascript
// 接受消息
self.addEventListener("message", function (e) {
  console.log(e.data);
  // 向主线程发消息
  self.postMessage("你好，大哥");
});
```


- 当然，主线程也可以接受来自 worker 线程的消息



```javascript
// 接受消息
myWorker.onmessage = function (e) {
  console.log(e.data);
};
```


- 因为 worker 线程创建后就会一直存在，需要手动关闭<br />
在主线程中关闭



```javascript
myWorker.terminate();
```

<br />在 worker 线程中关闭<br />

```javascript
self.close();
```


<a name="b4d3c72e-1"></a>
## 特点


- 主线程与 Worker 之间的通信内容，可以是文本，也可以是对象。需要注意的是，这种通信是拷贝关系，即是传值而不是传址，Worker 对通信内容的修改，不会影响到主线程。
- 当传递较大的数据时 JavaScript 允许主线程把二进制数据直接转移给子线程，但是一旦转移，主线程就无法再使用这些二进制数据了，这是为了防止出现多个线程同时修改数据的麻烦局面。这种转移数据的方法，叫做[Transferable Objects](https://developer.mozilla.org/zh-CN/docs/Web/API/Transferable)。



```
// Transferable Objects 格式
worker.postMessage(arrayBuffer, [arrayBuffer]);

// 例子
var ab = new ArrayBuffer(1);
worker.postMessage(ab, [ab]);
```


<a name="0ab2d7b6"></a>
### worker 线程的全局对象不是 window，因此定义在 window 上面的对象和方法不可以使用，worker 全局属性和方法


- self.name： Worker 的名字。该属性只读，由构造函数指定。
- self.onmessage：指定 message 事件的监听函数。
- self.onmessageerror：指定 messageerror 事件的监听函数。发送的数据无法序列化成字符串时，会触发这个事件。
- self.close()：关闭 Worker 线程。
- self.postMessage()：向产生这个 Worker 线程发送消息。
- self.importScripts()：加载 JS 脚本。



<a name="c931653c"></a>
## 场景


- worker 线程完成轮询
