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

