目前最盛行的模块加载器就是 CommonJS 、AMD（Require.js）、UMD、ES6 的 Module（PS:以及可能成为 JavaScript 下一个版本的 ES Harmony）。今天，我们就简单地来了解一下 CommonJS 和 AMD。

## CommonJS

CommonJS 模块由两个基础的部分构成：一个是取名为 exports 的自由变量（其实就是 module 的属性），通过这个 exports 属性来实现对外暴露的对象或属性。

例如：
a.js 中定义一个对象
```javascript
let a = {
	say: function() {
		console.log('hello world')
	}
}
let b = function() {
	console.log('hhhhh')	
}

module.exports.a = a
module.exports.b = b
```

b.js 中引用并使用 a.say 和 b
```javascript
const mA = require('a.js')

// 调用 a 对象的 say 方法
mA.a.say()
// 调用 b 方法
mA.b()
```
从上面的代码，我想应该更容易看出 module 其实就是一个对象，通过 exports 来往这个对象上添加属性，require 后就可以通过这个对象访问对于的属性。

不过需要注意的是 CommonJS 模块的特点：
1. 模块代码只会在模块作用域中运行，防止了全局作用域的污染
2. 并且模块的代码可以多次加载，但是需要注意的是只有在第一次运行时会加载并缓存，之后运行则会调用缓存
3. 模块代码加载的顺序，就是代码中加载的顺序

module 对象的构造函数是这样的：
```javascript
function Module(id, parent) {
	this.id = id
	this.exports = {}
	this.parent = parent
	...
}
```

例如：
```javascript
var jquery = require('jquery')
export.$ = jquery
console.log(module)
```

```javascript
{
	id: '.',
	exports: { '$': [Function] },
	parent: null,
	filename: '/path/to/jquery.js',
	loaded: false,
	children:
	[{
		id: '/path/to/no_modules/jquery/dist/jquery.js',
		exports: [Function],
		parent: [Circular],
		filename: '/path/to/node_modules/jquery/dist/jquery.js',
		loaded: true,
		children: [],
		paths: [Object]
		....
	}]
}
```

## AMD

需要注意 CommonJS 规范加载模块是同步。而 AMD（异步模块定义 Asynchronous Module Definition） 规定则是非同步加载模块，允许指定回调函数。

AMD 主要是由一个帮助定义模块的 define 方法和一个处理依赖加载的 require 方法组成。
1.define 方法
```javascript
define(
	module_id,
	[dependencies],
	definition function
)
```

其中，dependencies 参数代表我们正在定义的模块需要的 dependency 数组，第三个参数是用来执行初始化模块的方法。
例如：
```javascript
define('module',
	["test1", "test2"],
	function (test1, test2) {
		var module = {
			say: function () {
				console.log('hello world')
			}
		}
	return function
})
```

AMD 是通过 define 方法定义模块
```javascript
define(['package/lib'], function(lib) {
	function foo() {
		console.log('hello wold')
	}

	return {
		foo: foo
	}
})
```
需要注意的是，当一个模块没有定义依赖数组的时候，这个模块就会被当做 CommonJS 来对待，即同步加载。

2.require 则被用来从一个顶级文件或模块里加载代码
例如
```javascript
require(["test1", "test2"], function(tes1, test2) {
	test1.say()
})
```