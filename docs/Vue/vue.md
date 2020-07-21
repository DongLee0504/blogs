# vue 学习笔记

<a name="ARgM6"></a>

# 指令

<a name="6soiP"></a>

## 1、v-if 与 v-show

使用 v-if 会有性能开销。每次插入或者移除元素时都必须要生成元素内部的 DOM 树，这在某些时候是非常大的工作量。而 v-show 除了在初始创建开销时之外没有额外的开销。如果希望频繁地切换某些内容，那么 v-show 会是最好的选择。
<a name="73gG8"></a>

# 响应式

vue data 的响应式是在 vue 实例初始化时添加了 getter/setter 方法，类似 js

```JavaScript
Object.defineProperty(obj, prop, descriptor)
```

```JavaScript
  var obj = {};
  var initValue = 'hello';
  Object.defineProperty(obj,"newKey",{
      get:function (){
          //当获取值的时候触发的函数
          return initValue;
      },
      set:function (value){
          //当设置值的时候触发的函数,设置的新值通过参数value拿到
          initValue = value;
      }
  });
  //获取值
  console.log( obj.newKey );  //hello

  //设置值
  obj.newKey = 'change value';

  console.log( obj.newKey ); //change value
```

# 路由

# 监听（watch）

- 监听 data 对象中的某个属性
  有些时候会将一个对象存储在 data 中，监听这个对象的某个属性

```JavaScript
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

```JavaScript
watch: {
  formData: {
    handler() {
      console.log(val, oldVale)
    },
    deep: true
  }
}
```

# 过滤器（filters）

- 表达式中可以采用链式写法使用多个过滤器

```JavaScript
{{ productCost | round | formateCost }}
```

- 过滤器传参
  输入的参数会以第二个参数传给过滤器函数

```JavaScript
{{ productCost | round('$')}}

filters: {
  round(val, type) {
    return
  }
}
```

- v-bind 也可以使用过滤器
  ```JavaScript
  <span v-bind:value="value | round('$')"></span>
  ```
- 注意事项
  - 不能使用 this 访问 data 中的数据或者方法（故意设计成这样，因为过滤器是纯函数，对于同样的输入返回同样的输出，不涉及外部数据，要想使用外部数据，可以通过参数传入）
  - vue 2 中只可以在插值或者 v-bind 中使用过滤器， vue 1 可以在任何地方使用

<a name="TEIuP"></a>

# nextTick

- 在 Vue 生命周期的`created()`钩子函数进行的 DOM 操作一定要放在`Vue.nextTick()`的回调函数中

在`created()`钩子函数执行的时候 DOM 其实并未进行任何渲染，而此时进行 DOM 操作无异于徒劳，所以此处一定要将 DOM 操作的 js 代码放进`Vue.nextTick()`的回调函数中。与之对应的就是`mounted()`钩子函数，因为该钩子函数执行时所有的 DOM 挂载和渲染都已完成，此时在该钩子函数中进行任何 DOM 操作都不会有问题 。

- 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的 DOM 结构的时候，这个操作都应该放进`Vue.nextTick()`的回调函数中。

解释：

> Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的 `Promise.then` 和`MessageChannel`，如果执行环境不支持，会采用 `setTimeout(fn, 0)`代替。

> 例如，当你设置`vm.someData = 'new value'`，该组件不会立即重新渲染。当刷新队列时，组件会在事件循环队列清空时的下一个“tick”更新。多数情况我们不需要关心这个过程，但是如果你想在 DOM 状态更新后做点什么，这就可能会有些棘手。虽然 Vue.js 通常鼓励开发人员沿着“数据驱动”的方式思考，避免直接接触 DOM，但是有时我们确实要这么做。为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用`Vue.nextTick(callback)` 。这样回调函数在 DOM 更新完成后就会调用。

> 参考文献
> [https://segmentfault.com/a/1190000012861862](https://segmentfault.com/a/1190000012861862)

<a name="qbgqW"></a>

# 生命周期钩子

1. **beforeCreate（创建前）**<br />完成实例初始化，<br />**初始化非响应式变量 this 指向创建的实例；**<br />可以在这加个 loadingEvent ；<br />数据计算的监视方法
1. **created（创建后）**<br />实例创建完成<br />数据（的数据道具计算）的初始化导入依赖项。<br />**可访问 data compute 的监视方法上的方法和数据**<br />**未挂载 DOM，无法访问$ el，$ ref 为空数组**<br />可在这结束加载，还要做一些初始化，实现函数自执行，<br />**可以对数据数据进行操作，可进行一些请求，请求不易过多，避免白屏时间太长。如果**<br />**在此阶段进行的 DOM 操作一定要放在 Vue 上。 nextTick（）的某些函数中**
1. **beforeMount（加载前）**<br />有了 el，编译了 template | / outerHTML<br />能找到对应的模板，并编译成 render 函数
1. **mount（加载后）**<br />完成创建 vm。$ el，和双向绑定，<br />完成挂载DOM和渲染；可在安装钩子对挂载的dom进行操作<br />即有了DOM并且完成了双向绑定可访问DOM例程，$ ref<br />可在这发起发起请求，拿回数据，配合路由钩子做一些事情；<br />可对 DOM 进行操作
1. **beforeUpdate（更新前）**<br />数据更新之前<br />可在更新前访问现有的 DOM，如手动删除的事件监听器；
1. **updated（更新后）**<br />完成虚拟 DOM 的重新渲染和打补丁；<br />组件 DOM 已完成更新；<br />并依赖的 dom 操作<br />**注意：不要在此函数中操作数据，会封闭死循环的。**
1. **beforeDestroy（销毁前）**<br />在执行 app。\$ destroy（）之前<br />可做一些删除提示，如：你确认删除 XX 吗？<br />**可用于销毁定时器，解绑时间销毁插件对象**
1. **破坏（销毁后）**<br />在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。你无法在其中进行任何操作。
1. ![aratar](https://cdn.nlark.com/yuque/0/2020/png/216639/1587974538401-f52d86f2-592b-42da-94a2-035e73344119.png)
   > 参考文献
   > [https://segmentfault.com/a/1190000011381906](https://segmentfault.com/a/1190000011381906)

# 事件处理

- 使用方法-- 事件对象最为第一个参数传入

```javascript
<div id="app">
    <button v-on:click="greet">点击</button>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      counter: 0
    },
    methods: {
      greet(e) {
        console.log(e) // 打印的是event事件
      }
    }
  })
</script>
```

- 使用内联代码-- 使用\$event

  ```javascript
  <div id="app">
    <button v-on:click="greet(1, $event)">点击</button>
  </div>

  <script>
  new Vue({
    el: '#app',
    data: {
      counter: 0
    },
    methods: {
      greet(type, e) {
        console.log(e)
      }
    }
  })
  </script>
  ```

# 自定义指令

1. 钩子函数

- bind 钩子函数会在指令绑定到元素时被调用。
- inseted 钩子会在绑定的元素被添加到父节点时被调用一一但和 mounted 一样，
  此时还不能保证元素已经被添加到 DOM 上。可以使用 this.\$nextTick 来保证
  这一点。
- update 钩子会在绑定该指令的组件节点被更新时调用，但是该组件的子组件可能
  此时还未更新。
- componentUpdated 钩子和 updated 钩子类似，但它会在组件的子组件都更新完成
  后调用。
- unbind 钩子用于指令的拆除，当指令从元素上解绑时会被调用。

```javascript
  <div id="app">
    <div v-demo="counter"></div>
  </div>

  <script>
  new Vue({
    el: '#app',
    data: {
      counter: 0
    },
    directives: {
      demo: {
        bind: function (el, binding, vnode) {
          var s = JSON.stringify
          el.innerHTML =
            'name: ' + s(binding.name) + '<br>' +
            'value: ' + s(binding.value) + '<br>' +
            'expression: ' + s(binding.expression) + '<br>' +
            'argument: ' + s(binding.arg) + '<br>' +
            'modifiers: ' + s(binding.modifiers) + '<br>' +
            'vnode keys: ' + Object.keys(vnode).join(', ')
        }
      }
    }
  })
</script>
```

> 输出
> name: "demo"
> value: 0
> expression: "counter"
> argument: undefined
> modifiers: {}
> vnode keys: tag, data, children, text, elm, ns, context, fnContext, fnOptions, fnScopeId, key, componentOptions, componentInstance, parent, raw, isStatic, isRootInsert, isComment, isCloned, isOnce, asyncFactory, asyncMeta, isAsyncPlaceholder

# 组件

![](https://cdn.jsdelivr.net/gh/DongLee0504/imgs/企业微信截图_20200420111509.jpg)

- 1.  数据流和.sync 修饰符
      数据通过 prop 从父组件传到子组件是<b>单向的</b>
      如果需要双向绑定，需要 sync
- 2.  插槽
      父组件向子组件传递数据（主要是 html 模板）
      默认插槽和具名插槽好理解，主要看下作用域(子组件插槽的数据要传递给父组件展示)

```javascript
<child>
  <!-- scope可以随便命名，只是得到插槽对象 -->
  <template v-slot="scope">
    {{scope.item}}
  </template>
</child>

<script>
  new Vue({
    el: '#app',
    data: {
      counter: 0
    },
    components: {
      Child: {
        data() {
          return {
            list: [1, 2, 3]
          }
        },
        template: '<ul> <li v-for="(item, index) in list" :key="index"><slot :item="item"></slot></li></ul>'
      }
    }

  })
</script>
```

- 参考 [https://juejin.im/post/5ef6d1325188252e75366ab5](https://juejin.im/post/5ef6d1325188252e75366ab5)

# vue-loader

1. 处理资源路径


    ``` javascript
    <img src="../image.png">
    ```
    将会编译为
    ``` javascript
    createElement('img', {
      attrs: {
        src: require('../image.png') // 现在这是一个模块的请求了
      }
    })
    ```
    相关loader： file-loader（打包时文件路径正确，相对路径部署时url正确），url-loader（小文件转换为base64,有效减少http请求）

2. scoped css
   当 style 是 scoped，p{color: red} 会慢很多，应以 class 或 id 代替
   深度作用选择器： >>>
3. css modules

# EventBus

```javascript
// main.js
Vue.prototype.$EventBus = new Vue();
```

```javascript
  // a.Vue
  methods:{
    addEvent() {
      this.$EventBus.$emit('addEvent', 1)
    }
  }
```

# \$attrs

- \$attrs 包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 `v-bind="$attrs"` 传入内部组件——在创建高级别的组件时非常有用。

```vue
<template name="component-name">
  <div>
    <child :name="person.name" :age="person.age"></child>
  </div>
</template>
<script>
export default {
  name: "Detail",
  data() {
    return {
      person: {
        name: "张三",
        age: "18",
        sex: "男",
      },
    };
  },
  components: {
    child: {
      template: '<div><grand v-bind="$attrs"></grand></div>',
      props: ["age"],
      components: {
        grand: {
          template: "<div></div>",
          created() {
            console.log("孙组件", this.$attrs);
          },
        },
      },
      created() {
        console.log("子组件", this.$attrs);
      },
    },
  },
};
</script>
```

![](https://cdn.jsdelivr.net/gh/DongLee0504/imgs/20200622144110.png)
\$attrs 可以访问到被绑定的属性（props 中申明的除外），通过`v-bind="$attrs"`可以将属性传递到孙组件

# require.context

- 当一个 js 里面需要手动引入过多的其他文件夹里面的文件时，就可以使用
- 在 Vue 项目开发过程中，我们可能会遇到这些可能会用到`require.context`的场景
  - 当我们路由页面比较多的时候，可能会将路由文件拆分成多个，然后再通过`import`引入到`index.js`路由主入口文件中
  - 开发了一系列基础组件，然后把所有组件都导入到`index.js`中，然后再放入一个数组中，通过遍历数组将所有组件进行安装。
  - 当使用`svg symbol`时候，需要将所有的 svg 图片导入到系统中（建议使用`svg-sprite-loader`）
- 常规操作
  ![](https://user-gold-cdn.xitu.io/2020/6/12/172a8dbf913be740?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
- `require.context`基本语法
  ![](https://user-gold-cdn.xitu.io/2020/6/12/172a900bd9c4737a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
- 通过`require.context`安装 Vue 组件
  ![](https://user-gold-cdn.xitu.io/2020/6/12/172a907e5f763528?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

# 自定义`.sync`

- 自定义`.sync`需要属性+事件（`update:属性名`）

  ```vue
  <template>
    <div>
      <MyMask :visible.sync="visible"></MyMask>
    </div>
  </template>
  <script>
  export default {
    data() {
      return {
        visible: true,
      };
    },
    components: {
      MyMask: {
        template: '<div v-if="visible" @click="handleClick">i am mask</div>',
        props: {
          visible: {
            type: Boolean,
            default: false,
          },
        },
        methods: {
          handleClick() {
            this.$emit("update:visible", false);
          },
        },
      },
    },
  };
  </script>
  ```

# hook

- 监听组件本身的生命周期

  ```js
  export default {
    mounted() {
      // 监听组件销毁
      this.$once("hook:beforeDestroy", () => {
        // doSomething
      });
    },
  };
  ```

- 监听子组件生命周期
  ```js
  <custom-select @hook:updated="$_handleSelectUpdated" />
  ```

# observable（轻量级 vuex）

- 创建 store

  ```es6
  import Vue from "vue";

  // 通过Vue.observable创建一个可响应的对象
  export const store = Vue.observable({
    userInfo: {},
    roleIds: [],
  });

  // 定义 mutations, 修改属性
  export const mutations = {
    setUserInfo(userInfo) {
      store.userInfo = userInfo;
    },
    setRoleIds(roleIds) {
      store.roleIds = roleIds;
    },
  };
  ```

- 组件中使用

  ```es6
  <template>
    <div>
      {{ userInfo.name }}
    </div>
  </template>
  <script>
  import { store, mutations } from "../store";
  export default {
    computed: {
      userInfo() {
        return store.userInfo;
      },
    },
    created() {
      mutations.setUserInfo({
        name: "子君",
      });
    },
  };
  </script>
  ```

# vue-x

1. state
   获取 state 的方法

```JavaScript
computed:{
counter() {
 return this.$store.state.count
}
}
```

辅助函数 mapState

```javascript
computed: mapState(['count']),

// 等同于
computed: mapState({
 count: state => state.count
}),
```

当 computed 里面的变量既依赖本地的 data 又依赖 store 的 state

```javascript
computed:{
 counter() {
   return this.data.xxx
 },
 ...mapState(['count'])
},
```

2.  getter
    就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。
3.  mutation

- <b>更改 Vuex 的 store 中的状态的唯一方法是提交 mutation</b>
- 同步变更 state
- 辅助函数
  组件中这样写，那么如何将 payload 传入 mutation

```javascript
methods: mapMutations(["addTodo"]);
```

这样就可以

```javascript
<button @click="addTodo({todo: {id: 1, text:'', done: true}})">addTodo</button>
```

4.  action
    - <b>action 处理异步操作</b>
    - 异步操作如何知道 action 什么时候完成 -- action 返回 promise


    ```javascript
    actions: {
      incrementAsync2({ commit }) {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            commit("increment");
            resolve()
          }, 5000);
        });
      }
    }
    ```
    组件中

    ```javascript
    incrementAsync2() {
      this.$store.dispatch('incrementAsync2').then(() => {
        console.log('action 完成')
      })
    }
    ```

# vue cli 2.x 字体丢失

- 2.x 版本 build 后无法找到字体
- 解决方案参考  
  [https://github.com/vuejs-templates/webpack/issues/166](https://github.com/vuejs-templates/webpack/issues/166)
