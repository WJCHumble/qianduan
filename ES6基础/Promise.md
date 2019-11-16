## Promise的基础知识
	
Promise相当于异步操作结果的占位符，它不会去订阅一个事件，也不会传递一个回调函数给目标函数，而是让函数返回一个Promise。

### Promise的生命周期

进行中（pending），也处于未处理（unsettled）   即此时异步操作尚未完成
已处理（setlled）   即异步操作执行结束
异步操作成功完成（Fullfilled）	   
异步操作未能成功完成（Rejected）    即由于程序错误或一些其他的原因，异步操作未能成功完成。

所有的Promise都有then()方法，它接收两个参数，第一个是当Promise的状态为fullfilled时要调用的函数，与异步操作相关的附加数据都会传递给这个完成函数。第二个是当Promise状态变为rejected时要调用的函数，所有失败状态相关的附加数据都会传递给这个拒绝函数。

Promise还有一个catch()，相当于只给其传入拒绝处理程序的then()方法。
```javascript
promise.catch(function(err) {
		console.error(err.message);
	})
promise.then(null, function(err) {
		console.log(err.message);
	})
```

注：then()方法和catch()方法一起使用才能更好地处理异步操作结果。
	
### 创建未完成的Promise

用Promise构造函数可以创建新的Promise，构造函数只接受一个参数：包含初始化Promise代码的执行器（executor）函数。
执行器接受两个参数：resolve()函数和reject()函数。即执行器成功时调用resolve()函数，反之，失败时则调用reject()函数。

需要注意的是：Promise具与任务编排(job scheduling)类似的工作原理，Promise的执行器会立即执行，然后才会执行后续流程中的代码
例如：
```javascript
let promise = new Promise(function(resolve, reject) {
			console.log("Promise");
			resolve();
		});

		promise.then(function() {
			console.log("Resolve.");
		})

		console.log("Hi!")
```

这段程序会输出:
```javascript
Promise
Hi!
Resolve.
```
即输入then()处于console.log()前面，但是Promise的完成和拒绝处理程序总是在执行器完成后被添加到任务队列的末尾。

### 创建已处理的Promise

即通过Promise的构造函数创建已处理的构造函数。
例如：  
```javascript
  let promise = Promise.resolve(42);
		promise.then(function(value) {
			console.log(value);
		})
		//reject的创建方式也类似，只不过promise.catch()不是then
```
注：如果向Promise.resolve()方法或Promise.reject()方法传入一个Promise，这个Promise会被直接返回。

#### 非Promise的Thenable对象
	
Promise.resolve()方法和Promise.reject()方法都可以接受非Promise的Thenable对象作为参数。（拥有then()方法并且接受resolve和reject这两个参数的普通对象就是非Promise的Thenable对象）

### 执行器错误

如果执行器内部抛出一个错误，且Promise的拒绝处理程序存在时，则Promise拒绝处理程序就会被调用。
例如：  
```javascript
let promise = new Promise(function(resolve, reject) {
				throw new Error("Explosion!");
			});

			promise.catch(function(error) {
				console.log(error.message);
			});
```

## 全局的Promise拒绝处理

全局的Promise的拒绝处理就是用于处理当Promise被拒绝，并且没有提供拒绝处理程序时触发改事件，或当Promise被拒绝时，拒绝处理程序被调用时，触发该事件。

## 串联Promise

当知识单独调用then()或catch()方法时实际上只是创建并返回了另一个Promise，只有当第一个Promise完成或被拒绝后，第二个Promise才会被解决。

### 捕获错误

```javascript
let p1 = new Promise(function(resolve, reject) {
	resolve(42);
});

p1.then(function(value) {
	throw new Error("Boom!");
}).catch(function(error) {
	console.log(error.message);
	throw new Error("Exception!");
}).catch(function(error) {
	console.log(error.message);
})	
//即可以通过串联的方式实现连续捕捉异常
```

### Promise链的返回值

即可以给下游的Promise传递数据。
例如：
```javascript
let p1 = new Promise(function(resolve, reject) {
	resolve(42);
});

p1.then(function(value) {
	console.log(value);
	return value + 1;
}).then(function(value) {
	console.log(value);
})
```

### Promise链中返回Promise

即在完成或解决处理程序中返回已创建好的Promise对象
```javascript
let p1 = new Promise(function(resolve, reject) {
	resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
	resolve(43);
});

p1.then(function(value) {
	//第一个完成处理程序
	console.log(value);
	return p2;    //因为之后执行的是完成处理程序  所以p2一定需要resolve(),reject再调用then就没用了。
}).then(function(value) {
	console.log(value);
})

//需要注意的是第二个完成处理程序并不是执行的p2这个Promise 而是执行的是另一个Promise
```

按顺序执行指定的Promise
即创建一个Promise，resolve或reject它，在完成或拒绝处理程序中创建新的Promise，并返回它，再在第一个的then完成处理程序中输出它
注：新建的Promise无论是resolve或reject，return返回后，都要对应相对的处理程序执行，否则不会执行！

## 响应多个Promise

创建多个Promise，通过Promise.all()或Promise.race()方法，实现监听多个Promise。

### Promsie.all()

Promise.all()只接收一个参数（含多个受监视的Promise的可迭代对象，例如数组），并返回一个Promise
注：当Promise.all()方法中的Promise只要有一个被拒绝，则返回的Promsie没等所有的Promise都完成就立即被拒绝，返回的value是拒绝的时候传入的参数。

### Promise.race()

Promise.race()则有所不同，它(value)是返回第一个处理程序，无论是reject()或resolve()都是返回第一个已完成的状态的Promise

## 自Promise继承

Promsie与其他内建类型一样，可以作为父类派生子类，即可以定义自己的Promise变量扩展内建Promise的功能。	
例如：
```javascript
class MyPromise extends Promise {
	//使用默认的构造函数
	success(resolve, reject) {
		return this.then(resolve, reject);
	}

	failure(reject) {
		return this.catch(reject);
	}
}
//即success()模仿resolve()方法  failure模仿reject方法
```

## 基于Promise的异步任务执行

注：需要结合迭代器的知识....未完待续