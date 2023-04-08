React Egghead.io

# The Beginner's Guide to React

使用 JS 创建一个 Hello World 的界面

![1680921297476](.\assets\1680921297476.png)



使用React API 创建并渲染元素

```js
const rootEl = document.getElementById('root');
const element = React.createElement('div', {
  children: 'Hello World',
  className: 'container'
})
ReactDOM.render(element, rootEl)
```

