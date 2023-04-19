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

## 组件Props传的值不对

> React v15.5 之后 需要下载 prop-types库才能使用

使用PropTypes来验证从props接收的任何数据

```js
const rootElement = document.getElementById('root')

function SayHello({firstName, lastName}) {
  return (
    <div>
      Hello {firstName} {lastName}!
    </div>
  )
}

SayHello.propTypes = {
  firstName: PropTypes.string,
  lastName: PropTypes.string,
}

const element = <SayHello firstName={false} />

ReactDOM.render(element, rootElement)
```

> 如果控制台没有报错，也许是因为你引入的是`production`版本的React而不是`development`版本！

## 二次渲染

如何获取当前时间

```js
const time = new Date().toLocaleTimeString()
```

如果想要实时更新时间

React的写法

```js
const rootElement = document.getElementById('root')
function tick(){
  const time = new Date().toLocaleTimeString()
  const element = (<div>
     <div>Current Time: </div>
     {time}
   </div>)
   ReactDOM.render(element, rootElement)
}
setInterval(tick, 1000)
```

原生的写法

```js
const rootElement = document.getElementById('root')
function tick(){
  const time = new Date().toLocaleTimeString()
  const element = `
    <div>Current Time:</div>
    ${time}
  `
  rootElement.innerHTML = element
}
setInterval(tick, 1000)
```



- 原生的是刷新整个body，从time到根元素
- React只会更新局部time

例如：页面上有两个输入框的时候，如果你用原生的写法，聚焦到某个元素上，下一秒页面刷新，聚焦消息；但是使用React，元素刷新不影响我的输入框聚焦。

原理是：你使用React元素的时候，如果你把它赋值给`ReactDOM.render`或者触发二次渲染，它会比较你这一次返回的渲染内容和上一次的渲染，对两者做一个diff，然后只会更新不同的部分。

## 混合样式

我们有一系列的css样式要运用，渲染三个不同大小和背景色 Box

创建了一个函数，接收className，与公共类box结合在一起

```
// 防止类名和传参 被覆盖/冲突
function Box({className, ...rest}){
	return <div className={`box ${className}`} {...rest} />
}
// 如果没有其他类 只有公共类 渲染的时候会有一个undefine的类
function Box({className='', ...rest}){
	return <div className={`box ${className}`.trim()} {...rest} />
}
// 增加一个属性
function Box({className='',style, ...rest}){
	return <div 
		className={`box ${className}`.trim()}
    style={{fontStyle: 'italic', ...style}}
		{...rest} 
	/>
}
// ...rest 表示用户还可以给Box加上 id 等其他属性
```

