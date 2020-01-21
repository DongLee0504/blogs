# vue 学习笔记

<a name="ARgM6"></a>

# 一、指令

<a name="6soiP"></a>

## 1、v-if 与 v-show

使用 v-if 会有性能开销。每次插入或者移除元素时都必须要生成元素内部的 DOM 树，这在某些时候是非常大的工作量。而 v-show 除了在初始创建开销时之外没有额外的开销。如果希望频繁地切换某些内容，那么 v-show 会是最好的选择。
<a name="73gG8"></a>

# 二、响应式

vue data 的响应式是在 vue 实例初始化时添加了 getter/setter 方法，类似 js

```
Object.defineProperty(obj, prop, descriptor)
```

# 三、路由

# 四、监听（watch）

- 监听 data 对象中的某个属性
  有些时候会将一个对象存储在 data 中，监听这个对象的某个属性

```
new Vue({
  data: {
    formData: {
      userName: ''
    }
  },
  watch:{
    'formData.userName' () {
      // this.formData.userName 变化了
    }
  }
})
```

- 监听整个对象

```
watch: {
  formData: {
    handler() {
      console.log(val, oldVale)
    },
    deep: true
  }
}
```

# 五、过滤器（filters）

- 表达式中可以采用链式写法使用多个过滤器

```
{{ productCost | round | formateCost }}
```

- 过滤器传参
  输入的参数会以第二个参数传给过滤器函数

```
{{ productCost | round('$')}}

filters: {
  round(val, type) {
    return
  }
}
```

- v-bind 也可以使用过滤器
  ```
  <span v-bind:value="value | round('$')"></span>
  ```
- 注意事项
  * 不能使用 this 访问 data 中的数据或者方法（故意设计成这样，因为过滤器是纯函数，对于同样的输入返回同样的输出，不涉及外部数据，要想使用外部数据，可以通过参数传入）
  * vue 2 中只可以在插值或者v-bind中使用过滤器， vue 1 可以在任何地方使用