# React渲染
React组件 -> ReactElement -> Fiber -> 渲染绑定 -> UI
## React组件
1. 函数组件
2. 类组件
## ReactElement
### 分类
1. 函数
2. DOM标签
## Fiber
### FiberRoot
```
export type FiberRoot = {
  containerInfo: any, // fiber节点的容器元素相关信息，通常会直接传入容器元素
  current: Fiber, // 当前fiber树中激活状态（正在处理）的fiber节点
  finishedWork: Fiber | null, // 已完成的工作正在进行的HostRoot已准备好提交
  context: Object | null, // 顶部上下文对象
  hydrate: Boolean, // 确定我们是否应该尝试在初始加载使用 hydrate
  nextScheduledRoot: FiberRoot | null // 多组件树FirberRoot对象以单链表存储链接，指向下一个需要调度的FiberRoot
}
```
### FiberNode
```
export type FiberNode = {
  tag: TypeOfWork, // 标记Fiber类型
  key: null | string // 唯一标识
  stateNode: any, // fiber所在组件树的根组件FiberRoot对象
  return: Fiber | null, // 处理完当前fiber后返回的fiber

  // fiber树结构相关链接
  child: Fiber | null,
  sibling: Fiber | null,
  index: number

  // ReactElement相关
  pendingProps: any, // 当前处理过程中的组件props对象
  memoizedProps: any, // 缓存的之前组件props对象

  // 更新相关
  updateQueue: UpdateQueue<any> | null,

  mode: TypeOfMode, // 描述fiber极其子树的属性

  effectTag: TypeOfSideEffect, // 基于质数相除的任务系统
}
```