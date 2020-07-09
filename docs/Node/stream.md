# 流的概念

- 流（stream）是 Node.js 中处理流式数据的抽象接口。 stream 模块用于构建实现了流接口的对象。

# 流的类型

- Writable - 可写入数据的流（例如 fs.createWriteStream()）。
- Readable - 可读取数据的流（例如 fs.createReadStream()）。
- Duplex - 可读又可写的流（例如 net.Socket）。
- Transform - 在读写过程中可以修改或转换数据的 Duplex 流（例如 zlib.createDeflate()）。

# Readable Stream

## Flowing Mode 模式

- 在 `Stream` 上绑定 `ondata` 方法就会自动触发这个模式
- ![](https://cdn.jsdelivr.net/gh/DongLee0504/imgs/node-stream-readable-flowing.png)
- 资源的数据流并不是直接流向消费者，而是先`push`到缓存池，缓存池有一个水位标记`highWatermark`，超过这个标记阈值，`push` 的时候会返回 `false` ，以下情况会返回 `false` 。
  - 消费者主动执行了 .pause()
  - 消费速度比 push 到缓存池的生产速度慢

## Non-Flowing Mode 模式

**stream 的默认模式** <br>

- \_readableState.flow = null，暂时没有消费者过来
- \_readableState.flow = false，主动触发了 .pause()
- \_readableState.flow = true，流动模式

# Writable Stream

![](https://cdn.jsdelivr.net/gh/DongLee0504/imgs/node-stream-writable.png)  
当生产者写入速度过快，把队列池装满了之后，就会出现「背压」，这个时候是需要告诉生产者暂停生产的，当队列释放之后，Writable Stream 会给生产者发送一个 drain 消息，让它恢复生产
# 参考
- [https://www.barretlee.com/blog/2017/06/06/dive-to-nodejs-at-stream-module/](https://www.barretlee.com/blog/2017/06/06/dive-to-nodejs-at-stream-module/)