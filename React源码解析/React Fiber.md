# React Fiber
React的执行分为Reconciler和Render两个阶段, 在Reconciler阶段React做纯Virtual DOM操作,在这个阶段我们可以把reconciler拆分为多个工作单元，React Fiber是如何分割/设置工作单元的，就是我们这章需要了解的。

## 为什么使用Fiber
当浏览器的主线程长时间忙于运行一些事情时,关键任务的执行可能被延迟。  
React 的一个核心概念是 UI 是数据的投影 ，组件的本质可以看作输入数据，输出UI的描述信息（虚拟DOM树）。  
也就是说，渲染一个 React app，其实是在调用一个函数，函数本身会调用其它函数，形成调用栈。前面我们已经讲到，递归调用导致的调用栈我们本身无法控制，
只能一次执行完成。而 Fiber 就是为了解决这个痛点，可以去按需要打断调用栈，手动控制 stack frame, 用循环代替递归
## Fiber概念
Fiber, 纤程，也叫协程，通常它会拿来跟线程对比，我们也拿它们对比理解其不同。
**纤程** 由纤程或者线程创建，其调度(非抢占式， 合作式的多任务)完全由代码控制
**线程** 由进程创建,其调度(抢占式的多任务)由内核控制，依照优先级执行任务

## Fiber(React中)是什么
Fiber 的基本目标是可以利用 scheduling（scheduling 即决定工作什么时候执行），即可以
1. 暂停工作，并在之后可以返回再次开始；
2. 可以为不同类型的工作设置优先级；
3. 复用之前已经完成的工作；
4. 中止已经不再需要的工作。
要达成以上目标，首先我们必须能把工作分成小单元。  

## Scheduling micro-tasks
我们需要把任务分成一个个子任务，在很短的时间里运行结束掉。可以让主线程先去做优先级更高的任务，然后再回来做优先级低的任务。
我们将会需要 requestIdleCallback() 函数的帮助。它在浏览器空闲时才执行callback函数，回调函数中 deadline 参数会告诉你还有多少空闲时间来运行代码，如果剩余时间不够，那么你可以选择不执行代码，保持了主线程不会被一直占用。
## 工作单元(UnitOfWork)实现
```
--- working asynchronously using requestIdleCallback -------------------------------------------------
| ------- Fiber ---------------    ------- Fiber ---------------    ------ Fiber ---------------     |
| | beginWork -> completeWork | -> | beginWork -> completeWork | -> |beginWork -> completeWork | ... |
| -----------------------------   ------------------------------    ----------------------------     |
------------------------------------------------------------------------------------------------------
                      ↓↓↓
-----------------------------------------------------------------------
| commitAllWork(flush side effects computed in the above to the host) |
-----------------------------------------------------------------------
```
code
```
const ENOUGH_TIME = 1
let workQueue = []
let nextWorkOfWork = null

function schedule(task) {
  workQueue.push(task)
  requestIdleCallback(performWork)
}

function performWork(deadline) {
  if (!nextUnitOfWork) {
    nextUnitOfWork = workQueue.shift()
  }

  while (nextUnitOfWork && deadline.timeRemaining() > ENOUH_TIME) {
    nextUnitOfWork = performUnitOfWork(nextUnitOfWork)
  }

  if (nextUnitOfWork || workQu.length > 0) {
    requestIdleCallback(performWork)
  }
}
```