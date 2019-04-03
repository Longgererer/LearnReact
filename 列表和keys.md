# 列表和keys
### 列表
`React`怎么将元素循环转化为列表呢？
````javascript
let arr = [1, 2, 3]
let result = arr.map( number => number * 2)
console.log(result)//[2, 4, 6]
````
上述代码是将数组用`map`遍历一遍，每一个数组中的元素都会执行`map`中的方法  
其实渲染`DOM`元素的方法是一样的：
````javascript
let arr = [1, 2, 3, 4, 5]
ReactDOM.render(
    <ul>
        {arr.map( number => <li key={number.toString()}>{number}</li> )}
    </ul>,
    document.getElementById('root1')
)
````
用`map`方法遍历`arr`数组，将数组每个元素都变成了`<li>{number}</li>`，在插入到`<ul>`中渲染进`DOM`

> key属性可以不写，但是会受到警告`a key should be provided for list items`也就是创建一个元素的时候必须包含一个key属性

此外，上面的例子也可以当成一个组件来渲染，方法都是一样的

### Keys
keys的作用是帮助`React`识别已经改变了的元素，每个元素都应当有自己独一无二的key标识，通常使用数据id作为元素的key
````javascript
arr.map( (list) => {
    <li key={list.id}>{list.text}</li>
})
````
但没有id的时候可以使用index，但不推荐这么做
````javascript
arr.map( (list, index) => {
    <li key={index}>{list.text}</li>
})
````
如果列表的顺序经常会变化的话，用index作为key会大大降低性能，还可能会引发组件的其他问题

**注意**： 键（Key）只是在兄弟节点之间必须唯一，数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的键：
````javascript
let arr = [
    {
        id: 1,
        title: '选项1'
    },
    {
        id: 2,
        title: '选项2'
    },
    {
        id: 3,
        title: '选项3'
    }
]
class ListElement extends React.Component {
    constructor(props) {
        super(props)
    }
    render() {
        return (
            arr.map( (arr) => <div key={arr.id}>{arr.title}</div> )
        )
    }
}
class List extends React.Component {
    constructor(props) {
        super(props)
    }
    render() {
        const arr = this.props.arr
        return (
            arr.map( (arr) => <ul key={arr.id}><ListElement/></ul> )
        )
    }
}
ReactDOM.render(
    <List arr={arr}/>,
    document.getElementById('root1')
)
````