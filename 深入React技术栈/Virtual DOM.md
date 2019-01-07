# Virtual DOM
在Web开发过程中，JavaScript可以通过修改DOM结构和内容来实现界面的显示和交互，但DOM的操作是十分消耗性能的，会导致浏览器内核的重绘和重排工作，虚拟DOM的出现就是为了减少这部分工作所带来的性能消耗。
## 什么是虚拟DOM
在React中render执行的结果并不是真正的DOM节点，而是一个轻量级的JavaScript对象，我们称之为虚拟DOM
```
const ReactElement = function(type, key, ref, self, source, owner, props) {
  const element = {
    $$typeof: REACT_ELEMENT_TYPE,
    type: type,
    key: key,
    ref: ref,
    props: props,
    _owner: owner,
  };

  // ...

  return element
}
```
## 虚拟DOM对比操作原生DOM的优势？
1. 在即时编译的时代，调用DOM的开销是很大的，而JavaScript的执行是在JavaScript引擎中，在现代浏览器内核实现中，JavaScript引擎都是独立的，这意味着DOM的操作都是DOM API的调用。
2. 我们知道Virtual DOM和原生DOM操作真正的优势在于大批量DOM节点的更新，这归功于它的batching 和diff，batching把所有的DOM操作搜集起来，一次性提交给真实的DOM。diff算法时间复杂度也从标准的的Diff算法的O(n^3)降到了O(n)。
PS: Virtual DOM对比操作原生DOM有优势但并不是说Virtual DOM比操作原生DOM快，相反Virtual DOM最终也是要操作原生DOM的，所以操作原生DOM无疑是最快的，而我们刚提到的两个优势真正要说明的是，虚拟DOM的优势在于将复杂的DOM操作逻辑放在了自己的实现里，而我们只需要关心我们需要的是个什么样的界面，这也是其核心思想：声明式。