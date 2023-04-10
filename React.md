React Egghead.io

# The Beginner's Guide to React

## 使用 JS 创建一个 Hello World 的界面

![1680921297476](.\assets\1680921297476.png)



## 使用React API 创建并渲染元素

```js
const rootEl = document.getElementById('root');
const element = React.createElement('div', {
  children: 'Hello World',
  className: 'container'
})
ReactDOM.render(element, rootEl)
```

## 使用JSX创建页面

- 不使用 `React.createElement` API 而是使用`JSX`语法，一种类似HTML的写法
- 需要引入`Babel`，实时解析JSX语法并渲染到浏览器中
- 注意使用babel的时候，需要将script类型设置为`text/babel`



### JSX 技巧

children可以作为一个props传入

```js
const className = 'container'
const myChildren = 'Hello World'
const element = <div className={className} children={myChildren} />
ReactDOM.render(element, rootElement)
```

也可以这样写

```js
const className = 'container'
const myChildren = 'Hello World'
const props = {className, myChildren}
const element = <div {...props} />
ReactDOM.render(element, rootElement)
```

添加新的"行内"属性替换`props`里属性

```js
const element = <div {...props} className="not-container" />
```



## 依次渲染两个元素

使用React.createElement

```js
const rootElement = document.getElementById('root')
const helloElement = React.createElement('span', null, 'Hello')
const worldElement = React.createElement('span', null, 'World')
const element = React.createElement(
  React.Fragment, 
  null, 
  helloElement,
  ' - ', 
  worldElement)
ReactDOM.render(element, rootElement)
```

使用JSX

```
const element = (
  <>
    <span>Hello</span> <span>World</span>
  </>
)
```

## 可复用组件

```jsx
// 可以把messgae看作一个函数
// 接收一个props传参 返回更多的React元素
const Message = props => <div className="message">{props.msg}</div>
const element = (
  <div>
    <Message msg="Hello World" />
    <Message msg="Goodbye World" />
  </div>
)
```

也可以等同于这种写法

```jsx
const Message = props => <div>{props.children}</div>
const element = (
  <div>
    <Message>Hello World</Message>
    <Message>Goodbye World</Message>
  </div>
)
ReactDOM.render(element, rootElement)
```

