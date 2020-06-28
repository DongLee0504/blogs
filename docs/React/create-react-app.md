## 设置环境变量

- 在根目录新建.env.development 和 .env.production 两个文件
- 环境变量必须以 REACT_APP_xx 形式
- 在组件代码可以 process.env.xx 获取变量

![](https://cdn.jsdelivr.net/gh/DongLee0504/imgs/20200628102435.png)

in env.development

```js
port=3006
REACT_APP_url=http://www.baidu.com
```

in component

```js
console.log(process.env.REACT_APP_url);
```

- [参考文档](https://facebook.github.io/create-react-app/docs/adding-custom-environment-variables)

## jsx 语法

> JSX 就是 Javascript 和 XML 结合的一种格式。React 发明了 JSX，可以方便的利用 HTML 语法来创建虚拟 DOM，当遇到<，JSX 就当作 HTML 解析，遇到{就当 JavaScript 解析.

## 组件

- 组件首字母大写
- 组件外层包裹原则：组件一般外层需要包裹个标签，如真不需要外层标签，需要将外层标签替换为<Fragment>
- 不要直接操作 state 数据

bad

```js
deleteItem(index){
    this.state.list.splice(index,1)
    this.setState({
        list:this.state.list
    })
}
```

good

```js
deleteItem(index){
    let list = this.state.list
    list.splice(index,1)
    this.setState({
        list:list
    })

}
```

## 坑

- jsx 注释
  bad

```jsx
<Fragment>
  //第一次写注释，这个是错误的
  <div>
    <input
      value={this.state.inputValue}
      onChange={this.inputChange.bind(this)}
    />
    <button onClick={this.addList.bind(this)}> 增加服务 </button>
  </div>
</Fragment>
```

good

```jsx
<Fragment>
  {/* 正确注释的写法 */}
  <div>
    <input
      value={this.state.inputValue}
      onChange={this.inputChange.bind(this)}
    />
    <button onClick={this.addList.bind(this)}> 增加服务 </button>
  </div>
</Fragment>
```

or

```jsx
<Fragment>
  {
    //正确注释的写法
  }
  <div>
    <input
      value={this.state.inputValue}
      onChange={this.inputChange.bind(this)}
    />
    <button onClick={this.addList.bind(this)}> 增加服务 </button>
  </div>
</Fragment>
```

- 标签中的 class 要换成 className
- label
  label 标签与 input 一起使用时， for 属性要改成 htmlFor

## 解析 html

- 标签要写 dangerouslySetInnerHTML={{__html: xxx}}
  code

```jsx
return (
      <div>
        <div dangerouslySetInnerHTML={{__html: this.state.html}}></div>
      </div>
)
```
