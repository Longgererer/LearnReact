# State和生命周期
这里我用的是官网的时钟例子，因为这个例子非常合适解释`State`
````javascript
function Clock(props) {
    return (
        <div>{props.time}</div>
    )
}
function date() {
    ReactDOM.render(
        <Clock time={new Date().toLocaleTimeString()}/>,
        document.getElementById('root1')
    )
}
setInterval(date, 1000)
````
这样虽然可以实现功能，但是如果多渲染几个时间的话，就要多启动一次计时器，怎么才能自动启动计时器呢？添加`State`实现这个功能

**State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。**

### 将函数组件转换成 class 组件
````javascript
class Clock extends React.Component {
    render() {
        return (
            <div>
                {this.props.time}
            </div>
        )
    }
}
````
然后将`props`中的`time`转移到`State`中
````javascript
class Clock extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            time: new Date().toLocaleTimeString()
        }
    }
    render() {
        return (
            <div>
                {this.state.time}
            </div>
        )
    }
}
ReactDOM.render(
    <Clock/>,
    document.getElementById('root2')
)
````
现在`props`转换成了`State`，但是还没有设置定时器，因为要先添加生命周期方法才行，当组件第一次被渲染到页面上的时候，就设置一个定时器，这在`React`中被称为**挂载**

当组件被删除/销毁的时候，应该清除定时器减少资源占用，这在`React`中被称为**卸载**

先创建一个方法`date`，用于改变`state.time`的值
````javascript
date() {
    this.setState({
        time: new Date().toLocaleTimeString()
    })
}
````

在`React`中，挂载对应的声明函数是`componentDidMount`，卸载对应的生命周期函数是`componentWillUnmount`，分别在组件挂载和卸载的时候执行，这些都是生命周期方法

在`componentDidMount`中设置定时器，因为它会在组件已经被渲染到DOM中之后运行，`componentWillUnmount`用于卸载后清除定时器
````javascript
componentDidMount() {
    this.timer = setInterval( () => {
        this.date()
    }, 1000)
}
componentWillUnmount() {
    clearInterval(this.timer);
}
````
这样就实现了时钟的功能

### 如何使用State
`setState`对新手不是很友好，因为`state`的更新很可能是异步的，这就导致写出来的代码可能会出很多`BUG`

官网是这样说的：

**不要直接修改state**  
直接修改`state`是不会导致`React`重新渲染组件的，而是使用`setState`

**state的更新可能是异步的**  
出于性能考虑，`React`可能会把多个`setState()`调用合并成一个调用。也就是说`setState`并不会马上改变`state`的值，而是排队等待其他的`setState`，所以在执行了`setState`之后马上访问`state`的值很可能还是改变之前的

**state的更新会被合并**
````javascript
constructor(props) {
    super(props)
    this.state = {
        a: '',
        b: ''
    }
}
componentDidMount() {
    this.changeStateA('XXX')
    this.changeStateB('XXXX')
}
changeStateA(data) {
    this.setState({
        a: data
    })
}
changeStateB(data) {
    this.setState({
        b: data
    })
}
````
这里的合并是浅合并，所以在`setState({a:data})`的时候，`b`的值是不会改变的，但是`a`的值会完全被`data`替换掉

**数据是向下流动的**  
不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是`class`组件

组件可以将`state`作为`props`传递给它的子组件

这通常会被叫做“自上而下”或是“单向”的数据流。任何的 `state`总是所属于特定的组件，而且从该 `state` 派生的任何数据或 `UI` 只能影响树中“低于”它们的组件。

在`React`中每个组件都是相互独立的
