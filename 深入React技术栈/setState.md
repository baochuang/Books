# React之SetState
我们知道React是通过管理状态来实现对组件的管理，而setState方法是React提供用来更新State的，当setState方法被调用的时候，React会重新调用render方法来重新渲染UI。  

## setState的更新策略
在React中状态的更新是有策略的，它们分别是批量更新模式和普通模式。  
普通模式下,setState能够即时更新state，重新调用 render 方法，然后把render方法所渲染的最新的内容显示到页面上。  
Batch模式下,React不会立刻修改state。而是把这个对象放到一个更新队列中，稍后才会从队列中把新的状态提取出来合并到 state中，然后再触发组件更新。

### 两种更新策略都是在什么条件下被执行？
React控制的事件处理过程采用批量更新模式即异步更新，相反则是普通模式即同步更新，有哪些是控制之外的呢，如通过JavaScript原生addEventListener直接添加的事件处理函数，使用setTimeout/setInterval。

## 批量更新模式的实现