## 事件轮询(是Node IO的基础核心)

指Node会先注册事件，随后不停地询问内核这些事件是否已经分发。当事件分发时，对应的回调函数就会被触发，如果事件没有分发时，继续执行之后的代码，直至该事件分发时则执行相应的回调函数。
例：
```javascript
	console.log('hello')
	setTimeout(function () {
		console.log('world')
		}, 5000)
	console.log('hi')

	//输出语句为  hello hi world
```

Node并发实现也实现了事件轮询。

## 单线程

即当一个函数执行时，不会有第二个函数也再执行。

## 高并发实现

当V8首次调用一个函数时，会创建一个众所周知的**调用堆栈**，或称为**执行堆栈**。即一个函数在调用时，又调用了另一个函数，V8就会把它添加到调用堆栈。
例如：
```javascript
	function a () {
		b();
	}
	function b () {}
```
此处的调用堆栈则为  a  b


HTTP服务器中
```javascript
	http.createServer(function () {
		a();
	});
	function a () {
		b()
	};
	function b () {};
```
Http请求到达服务器，Node会分发一个通知，调用堆栈为 a b

## 堆栈追踪

指在JS中，当错误发生时，在错误信息中可以看到一系列函数的调用。

## 小结

在开发当中应尽量避免使用同步IO，学会使用非阻塞IO和事件轮询。