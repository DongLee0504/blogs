# align-items 与 align-content

- 两个属性都是定义交叉轴如何对齐
- align-items 适用单行
- align-content 适用多行

# flex-grow 与 flex-shrink

- flex-grow 默认值为 0，即使空间有空余，也不会放大
- flex-shrink 默认值为 1，当空间不足时，会自动缩小。
- 为使 item 项能够弹性缩放，采用简写形式 flex: 1，即：flex-grow: 1，flex-shrink: 1

```CSS
.item {
      flex: 1;
}
```
# 参考链接
- [语雀](https://www.yuque.com/fe9/basic/tlk8ck#b2c744d3)
- [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
- [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
