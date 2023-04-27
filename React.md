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

公共类和自定义类、自定义行内样式等样式的结合

```jsx
function Box({className='', ...rest}){
	return <div 
		className={`box ${className}`.trim()}
		// 这里直接赋值给style 那么会根据行内样式和剩下的props(rest)的先后顺序 其中一个会被覆盖
    // fontStyle 或者 backgroundColor
    style={{fontStyle: 'italic'}}
		{...rest} 
	/>
}
// 解决方法是 将style传参，然后在Box方法里 将style进行拼接
// style={{fontStyle: 'italic', ...style}}
```

```jsx
const element = (
  <div>
    <Box size="small" style={{backgroundColor: 'lightblue'}}>
      small lightblue box
    </Box>
    <Box size="medium" style={{backgroundColor: 'pink'}}>
      medium pink box
    </Box>
  </div>
)
```

> ...rest 表示用户还可以给Box加上 id 等其他属性

## 事件处理

```js
// 给button添加绑定事件
<button onClick={()=>{setState({eventCount: state.eventCount + 1})}}>Click Me</button>
```

也可以将事件独立出来

```js
function setClick(){
  setState({eventCount: state.eventCount + 1})
}
<button onClick={setClick}>Click Me</button>
```

给input添加事件，并且将输入值赋值给username

```js
<input onBlur={(event)=>setState({username: event.target.value})} />
```

## State

```js
// 点击 name 聚焦在相应的 input 上， 使用htmlFor属性， 类似原生的label标签的for
<label htmlFor="name">Name:</label>
<input id="name" />
```

React has 'React Hook' for maintaining state for a component

```jsx
function Greeting(){
  // React Hook仅能在函数内使用
  const [name, setName] = React.useState('')
  const handleChange = event => setName(event.target.value)
  return (
    <div>
      <form>
        <label htmlFor="name">Name:</label>
        <input id="name" onChange={handleChange} />
        {name ? <strong>Hello {name}</strong>: 'Please type your name'}
      </form>
    </div>)
}
```

## Side effect

在输入框中输入某些值，然后将它存储在 localStorage 中；刷新页面，值就会被从 localStorage 中取出，然后渲染到 input 中。

> 这就是我们称之为 Side Effect(副作用)。

> 怎么理解React中的side-effect？
>
> 首先，pure function(纯函数)是指
>
> - 給定相同的輸入(傳入值)，一定會回傳相同輸出值結果(回傳值)
> - 不會產生副作用
> - 不依賴任何外部的狀態
>
> 这个在 React 中就是给一个 pure component 相同的 props，永远渲染出相同的界面，没有副作用。
>
> 要一個很清楚的概念是，Side Effect(副作用)並不是指"好"或者"不好"的意思，而是它有可能會影響到其他環境的使用情況。

1.使用 `React.useEffect` ，将函数中存储的name 设置到localStorage中的name

2.为了初始化获取到localStorage中的name，使用`window.localStorage.getItem('name')`，如果没有就初始化为空字符串

3.为了确保input展示的是我们存储的name，给input设置了`value`属性

```jsx
function Greeting(){
  const [name, setName] = React.useState(window.localStorage.getItem('name')|| '')
  React.useEffect(()=>{
    window.localStorage.setItem('name', name)
  })
  const handleChange = event => setName(event.target.value)
  return (
    <div>
      <form>
        <label htmlFor="name">Name:</label>
        <input id="name" onChange={handleChange} value={name} />
        {name ? <strong>Hello {name}</strong>: 'Please type your name'}
      </form>
    </div>)
}
```



## lazy initialization 延迟初始化/惰性初始化

```jsx
const [name, setName] = React.useState(window.localStorage.getItem('name')|| '')
```

> 上面这段代码，伴随着输入的时候，页面会重新渲染。
>
> 我们只需要在输入完成后，读取并存储到 localStorage 中，而不是每次重新渲染都读一次。

React useState 提供了一个延迟初始化的特性：你可以提供一个 function 作为初始值，这样它就只会执行一次，避免不必要的性能损耗，而不是每次 re-rendered 时候执行。

```jsx
const [name, setName] = React.useState(() => {
  return window.localStorage.getItem('name')|| ''
})
```



## Use effect deps

```jsx
    const rootElement = document.getElementById("root");
    function Greeting() {
      const [name, setName] = React.useState(() => {
        return window.localStorage.getItem("name") || "";
      });
      React.useEffect(() => {
        // 看这里👇 点击一按钮 就会打印一次
        console.log('Greeting useEffect')
        window.localStorage.setItem("name", name);
      });
      const handleChange = (event) => setName(event.target.value);
      return (
        <div>
          <form>
            <label htmlFor="name">Name:</label>
            <input id="name" onChange={handleChange} value={name} />
            {name ? <strong>Hello {name}</strong> : "Please type your name"}
          </form>
        </div>
      );
    }
    function App() {
      const [count, setCount] = React.useState(0);
      return (
        <>
        <button onClick={() => setCount((c) => c + 1)}>{count}</button>
          <Greeting />
        </>
      );
    }
    ReactDOM.render(<App />, rootElement);
```

> 每次点击按钮的时候，会触发App组件重新渲染，与此同时会触发Greeting组件的渲染，然后会调用Greeting其中的useEffect。实际上这个 side effect 是不需要运行的，因为 name 没有改变。(which means it was called more than it to be)

React.useEffect 提供了第二个参数，去优化这个问题。第二个参数叫 dependcyArray，你可以传入所有的你的sideEffect需要的依赖。

```jsx
React.useEffect(() => {
  console.log('Greeting useEffect')
  window.localStorage.setItem("name", name);
}, [name]); // 👈
```

React提供了一个 eslint-plugin-react-hooks 的ESLint插件，用来检查hooks是否被正确使用，是否缺少依赖项。

> 尽管effect callback被调用的次数超过了本需要的的次数，不是说它是一个bug，这只是一个可以优化程序运行速度的点。

