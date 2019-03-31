# 组件&props
定义组件：  
````javascript
function cpn(props){
    return (
        <div>
            <p>{props.name}</p>
        </div>
    )
}
````
> 该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

`ES6`的`class`语法也可以定义组件，但是不够函数定义组件来的简洁,而且可能会出现`this`指向问题：

````javascript
class cpn extends React.Component{
    render(){
        return (
            <div>
                <p>{this.props.name}</p>
            </div>
        )
    }
}
````

### 组件的渲染
组件是可以自定义标签的，比如：
````javascript
const element = <Item-list>{data}</Item-list>
````
**注意**：组件的第一个字母一定要**大写**，这是`React`的规定，因为它会把小写开头的标签视为原生`DOM`标签 

````javascript
function ItemList(props){
    return (
        <div>
            <p>{props.data}</p>
        </div>
    )
}
const element = <ItemList data='hello'/>
ReactDOM.render(
    element,
    document.getElementById('root5')
)
````
这样就可以渲染出组件了
>1. 创建一个用于创建组件的函数，名为`ItemList`
>2. 定义一个组件`ItemList`，给了组件一个`data`属性
>3. 执行`ReactDOM.render()`并将组件和根节点传入
>4. `React`调用`ItemList`，并将`{data: 'hello'}`对象作为`props`传入
>5. `ItemList`组件将`<div><p>hello</p></div>`元素作为返回值
>6. `React DOM`将`DOM`高效地更新为`<div><p>hello</p></div>`

上述这种创建组件的方法叫做**无状态函数式组件**，它有下列特点：
>* 组件不会被实例化，整体渲染性能得到提升
>* 组件不能访问this对象
>* 组件无法访问生命周期的方法
>* 无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用

### 组合组件
组件中可以引用其他组件作为子组件  
````javascript
function ItemList(props){
    return (
        <div>
            <p>{props.data}</p>
        </div>
    )
}
function App(){
    return (
        <div>
            <ItemList data='hello'/>
            <ItemList data='world'/>
            <ItemList data='!'/>
        </div>
    )
}
ReactDOM.render(
    <App/>,
    document.getElementById('root5')
)
````
一个`App`引用多个`ItemList`做子组件

### 提取组件
如果一个组件太过于复杂，就要把其中的一些部分提取出来，拆分成更小的组件  

````javascript
function header(props){
    return (
        <div className="header">
            <div className="header-img">
                <image src={props.avatarUrl}/>
            </div>
            <div className="header-name">
                <span>{props.user.nickName}</span>
            </div>
            <div className="header-create-time">
                {props.user.createTime}
            </div>
            <div className="header-active">
                <p className="header-active-content">
                    {props.user.text}
                </p>
            </div>
        </div>
    )
}
````
假设上面就是一个显示动态功能的组件，因为组件中包含了很多标签和嵌套，这给组件的重复使用造成了困难，所以应该拆分成更小的组件  

比如把头像单独提取成一个组件：
````javascript
function HeaderImg(props){
    return (
        <div className={props.imgClass}>
            <image src={props.url}/>
        </div>
    )
}
function Header(props){
    return (
        <div className="header">
            <HeaderImg imgClass="header-img" url={props.user.avatarUrl}/>
            <div className="header-name">
                <span>{props.user.nickName}</span>
            </div>
            <div className="header-create-time">
                {props.user.createTime}
            </div>
            <div className="header-active">
                <p className="header-active-content">
                    {props.user.text}
                </p>
            </div>
        </div>
    )
}
````
这样就将头像提取成组件了，按照上面的方法继续拆分其他的：
````javascript
function HeaderImg(props){
    return (
        <div className={props.imgClass}>
            <img src={props.url}/>
        </div>
    )
}
function HeaderName(props){
    return (
        <div className={props.nameClass}>
            <span>{props.name}</span>
        </div>
    )
}
function CreateTime(props){
    return (
        <div className={props.ctTimeClass}>
            <span>{props.ctTime}</span>
        </div>
    )
}
function HeaderActive(props){
    return (
        <div className={props.actClass}>
            <p className="header-active-content">
                {props.text}
            </p>
        </div>
    )
}
function Header(props){
    return (
        <div className="header">
            <HeaderImg imgClass="header-img" url={props.user.avatarUrl}/>
            <HeaderName nameClass="header-name" name={props.user.nickName}/>
            <CreateTime ctTimeClass="header-create-time" ctTime={props.user.createTime}/>
            <HeaderActive actClass="header-active" text={props.user.text}/>
        </div>
    )
}
````
虽然代码量多了不少，但是维护更加容易，还能多次复用
> react的图片标签是\<img>，不是\<image>，之前我用\<image>浏览器就报错，不知道为什么，研究好一会才发现

### 只读的Props
很重要的是：**Props是只读的**
只读就意味着`React`组件无论是用什么方法创建的组件（函数还是class）都不能改变自身的`props`

#### 纯函数
````javascript
function sum(a, b) {
    return a + b
}
````
像这种不会更改传入参数值的函数叫做纯函数，相反：
````javascript
function sum(a, b) {
    a --
}
````
这种更改传入参数的函数就不是纯函数

**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**