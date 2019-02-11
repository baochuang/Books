# React数据流
在React中，数据的流向是单向的——从父节点传递到子节点。如果顶层组件的某个prop改变了，React会递归的向下便利整棵组件树，重新渲染所有使用这个属性的组件。
```
import React, { Component } from 'react'

class TodoList extends Component {
    render() {
        <div>
            <div>
                <form>
                    <input />
                    <button>Add Task</button>
                </form>
            </div>
        </div>
    }
}
```
绑定事件
```
<form onSubmit={this.props.addItem}>
```
TodoList组件调用传值, addItem实现
```
class App extends Component {
    addItem = () => {
        console.log('Add Item Success')
    }

    render() {
        return <TodoList addItem={this.addItem}>
    }
}
```
此时我们的addItem
## 反向数据流

## 双向绑定