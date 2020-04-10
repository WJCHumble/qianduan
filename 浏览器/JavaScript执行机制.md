# JavaScript 执行机制

## 变量提升

变量提升，指在 JavaScript 代码执行过程中，JavaScript 引擎把变量的声明部分和函数的声明部分提升到代码开头的**行为**。而此时，变量的默认值为 undefiend
例如：
```javascript
showName() // {1}
console.log(myname) // {2}
var myname = 'wujingchang' // {3}
function showName() { // {4}
	console.log('showName()已执行')
}
// 打印出
// showName()已执行
// undefined
```
这段代码在 JavaScript 引擎执行时会是这样：
```javascript
// 变量提升
var myname = undefined
function showName() {
	console.log('showName()已执行')
}
// 代码执行
showName()
console.log(myname)
myname = 'wujingchang'
```

### JavaScript 代码执行流程

虽说 JavaScript 是解释型的语言，但是在执行 JavaScript 代码时。JavaScript 会对代码进行一次简单的编译，然后再对代码进行执行。

而编译的过程会发生：
- 确定执行上下文，即词法环境、变量环境
- 确定可执行代码

从原理角度讲解 JavaScript 变量提升过程发生了什么（还是上面那段代码）
```javascript
showName() // {1}
console.log(myname) // {2}
var myname = 'wujingchang' // {3}
function showName() { // {4}
	console.log('showName()已执行')
}
// 打印出
// showName()已执行
// undefined
```
编译阶段：
- {1} 和 {2} 都是执行代码，JavaScript 引擎划分为执行代码
- {3} 是变量声明，此时 JavaScript 引擎会在变量环境中创建变量 `myname` 并赋值为 `undefined`
- {4} 是函数声明，此时 JavaScript 引擎会将该函数体存储在堆空间中，然后在变量环境中创建变量 `showName` 并指向堆中间的函数体

执行阶段：
- 执行 {1}，JavaScript 引擎在变量环境中查找 `showName`，然后调用它的引用，即调用堆空间中的函数体打印出**showName()已执行**
- 执行 {2}，JavaScript 引擎在变量环境中查找 `myname`，打印出它的值，此时它的值为 `undefined`


## 调用栈

## 块级作用域

## 作用域和闭包

## this

