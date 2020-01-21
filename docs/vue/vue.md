# vue 学习笔记

<a name="ARgM6"></a>
# 一、指令
<a name="6soiP"></a>
## 1、v-if 与 v-show
使用v-if会有性能开销。每次插入或者移除元素时都必须要生成元素内部的DOM树，这在某些时候是非常大的工作量。而v-show除了在初始创建开销时之外没有额外的开销。如果希望频繁地切换某些内容，那么v-show会是最好的选择。
<a name="73gG8"></a>
# 二、响应式
vue data 的响应式是在vue实例初始化时添加了getter/setter方法，类似js
```
Object.defineProperty(obj, prop, descriptor)
```
# 三、路由