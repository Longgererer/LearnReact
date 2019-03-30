# JSX
`JSX` 就是 `javascript + xml` 的结合，它是 `javascript` 对象，会构建创建一个 `js` 对象来描述 `HTML` 结构的信息， `JSX` 是 `js` 的一种扩展语言，类似 `HTML` 但是又不是 `HTML` ，因为 `JSX` 中还能进行运算，判断，加入一些 `js` 语言等。 
 
 `JSX` 是 `React` 的核心组成部分，它使用 `XML` 标记的方式去直接声明界面，界面组件之间可以互相嵌套。可以理解为在 `JS` 中编写与 `XML` 类似的语言,一种定义带属性树结构（`DOM` 结构）的语法，它的目的不是要在浏览器或者引擎中实现，它的目的是通过各种编译器将这些标记编译成标准的 `JS` 语言。  
 
 根据官网的说法，`React` 并没有强制使用者一定要使用 `JSX，但是在 `JavaScript` 代码中将 `JSX` 和 `UI` 放在一起时，会在视觉上有辅助作用。它还可以使 `React` 显示更多有用的错误和警告消息。
### 使用JSX
> 在使用`JSX`之前，先要了解`ReactDOM.render()`

`ReactDOM.render()`接受三个参数，`JSX, CONTAINER, CALLBACK`  
>* JSX: react虚拟元素
>* CONTAINER: 容器，承载虚拟元素的容器
>* CALLBACK: 当我们将元素放到页面中呈现触发的函数

> 在 `JSX` 中可以在大括号内放置任何有效的 `JavaScript` 表达式,例如，`2 + 2`，`user.firstName` 或 `formatName(user)` 都是有效的 `JavaScript` 表达式。 

````javascript
let data = 'hello world'
const element = <div>{data}</div>
function name(){
    return 'hello world'
}

ReactDOM.render(
    <div>{data}</div>,
    document.getElementById('root1')
)
ReactDOM.render(
    <div>{name()}</div>,
    document.getElementById('root4')
)
ReactDOM.render(
    <div>hello world</div>,
    document.getElementById('root2')
)
ReactDOM.render(
    element,
    document.getElementById('root3')
)
````
效果：  
![截图未命名.jpg](https://i.loli.net/2019/03/30/5c9ed7760c4d8.jpg)

> `JSX` 也是一个表达式,在编译之后，`JSX` 表达式会被转为普通 `JavaScript` 函数调用，并且对其取值后得到 `JavaScript` 对象。

`JSX` 标签里能够包含很多子元素,假如一个标签里面没有内容，你可以使用 `/>` 来闭合标签，就像 `XML` 语法一样:
````javascript
const element = <img src={user.avatarUrl} />
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
)
````

`Babel` 会把 `JSX` 转译成一个名为 `React.createElement()` 函数调用

````javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
````
````javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
````
上面两个代码效果相等

`React.createElement()`实际上创造了一个对象：

````javascript
const element = <div className='ele' id='ele' title='ele'>
                    hello
                    <p>world</p>
                </div>
console.log(element)
````
后台输出：  
![截图未命名.jpg](https://i.loli.net/2019/03/30/5c9ee0077918c.jpg)

把一些暂时没用的信息去掉可以变成这样：
````javascript
const element = {
    type: 'div',
    props: {
        className: 'ele',
        id: 'ele',
        title: 'ele',
        children: [
            'hello',
            {
                type: 'p',
                props: {
                    children: 'world'
                }
            }
        ]
    }
}
````

这些对象被称为 “`React` 元素”。它们描述了你希望在屏幕上看到的内容。`React` 通过读取这些对象，然后使用它们来构建 `DOM` 以及保持随时更新。