# 数据结构

## JavaScript中的数组

### 创建
```
var arr = new Array(1, 2, 3) // [1, 2, 3]
arr = new Array(3) // [undefined, undefined, undefined]
arr.length // 3
arr.length = 0 // []
```
### 增
```
var arr =  [1, 2]
arr.push(3) // [1, 2, 3] return length
arr.unshift(0) // [0, 1, 2, 3] return 4
arr.concat([4, 5]) // [0, 1, 2, 3] return [0, 1, 2, 3, 4, 5] 不会对数组对象本身做修改
```
### 删
```
var arr = [0, 1, 2, 3, 4, 5]
arr.pop() // [0, 1, 2, 3, 4] return 5
arr.shift() // [1, 2, 3, 4] return return 0
arr.slice(1) // [1, 2, 3, 4] return [2, 3, 4] 不会对数组对象本身做修改
arr.splice(-1) // [1, 2, 3] return [4]
arr.splice(1) // [1] return [2, 3]
```
#### 清空
```
arr.length = 0
arr = []
```
### 改
```
arr[index] = value
```
### 查
```
var arr = [1, 2, 3, 4]

for (let i = 0，len = arr.length; i< len; i++) {

}

arr.forEach(function(item, index) => {

})

arr.map((item, index) => {

})

var children = document.body.children
Array.prototype.forEach.call(children, (item, index) => {

})

let j
for (j in arr) {

}

for (j of arr) {

}
```
#### 中断遍历

## 栈
* **特性** 先进后出(后进先出)

### 方法
* **push** 添加一个元素到栈顶
* **pop** 弹出栈顶元素
* **top** 返回栈顶元素
* **isEmpty** 判断栈是否为空
* **size** 返回栈里面元素的个数
* **clear** 清空栈

### 实现
```
const Stack = () => {
    const collections = Symbol('collections')
    
    class Stack {
        constructor() {
            this[collections] = []
        }

        push(item) {
            this[collections].push(item)
        }

        pop() {
            this[collections].pop()
        }

        top() {
            if (this[collections].length > 0 ) {
                return this[collections].slice(-1)[0]
            }
            return null
        }

        isEmpty() {
            return this[collections].length === 0
        }

        size() {
            return this[collections].length
        }

        clear() {
            this[collections].length = 0
        }
    }

    return new Stack() 
}
```
