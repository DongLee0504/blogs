# combineReducers(reducers)

- 多个不同的 reducer 函数以`object`参数传入`combineReducers`进行合并,返回`state`对象。返回的`state`对象会将传入的每个`reducer`返回的`state`按照对应的 key 命名
  ```js
  rootReducer = combineReducers({potato: potatoReducer, tomato: tomatoReducer})
  // rootReducer 将返回如下的 state 对象
  {
    potato: {
      // ... potatoes, 和一些其他由 potatoReducer 管理的 state 对象 ...
    },
    tomato: {
      // ... tomatoes, 和一些其他由 tomatoReducer 管理的 state 对象，比如说 sauce 属性 ...
    }
  }
  ```
  **此时 state 结构发生了改变，合并之前是每个 reducer 的`state`，合并后变成了对象**
- 每个传入 `combineReducers` 的 `reducer` 都需满足以下规则：
  - 所有未匹配到的 `action` ，必须把它接收到的第一个参数也就是那个 `state` 原封不动返回。
  - 永远不能返回 `undefined` 。当过早 `return` 时非常容易犯这个错误，为了避免错误扩散，遇到这种情况时 `combineReducers` 会抛异常
  - 如果传入的 `state` 就是 `undefined` ，一定要返回对应 `reducer` 的初始 `state` 。根据上一条规则，初始 `state` 禁止使用 `undefined` 。使用 `ES6` 的默认参数值语法来设置初始 `state` 很容易，但你也可以手动检查第一个参数是否为 `undefined` 。

# mapStateToProps & mapDispatchToProps

- 参考 [Redux 入门教程（三）：React-Redux 的用法](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

# connect

- UI 组件生成容器组件（ UI 组件负责渲染，容器组件负责业务逻辑）
- connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
- 只注入 dispatch，不监听 store
  ```
  export default connect()(TodoApp)
  ```
- 参考 [https://cn.redux.js.org/docs/react-redux/api.html](https://cn.redux.js.org/docs/react-redux/api.html)
