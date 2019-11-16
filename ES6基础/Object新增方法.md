## 新增方法

### Object.is()

在JavaScript中比较两个值，通常会通过相等运算符"=="或全等运算符"==="，使用它们会存在着不准确的运算
ES6中引入了Object.js()方法弥补了全等运算符的不准确运算，这个方法接收两个参数，即这两个参数类型相同且具有相等的值，则返回true，否则返回false
```javascript
console.log(Object.is(+0, -0));	//false

console.log(Object.is(NaN, NaN));	//true

console.log(Object.is(5, "5"));	//false
```

## Object.assign()

ES6提供了Object.assign()实现对象间属性的传递
```javascript
function EventTarget() {

}

EventTarget.prototype = {
	constructor: EventTarget,
	emit: function(str) {
		console.log(str);
	},
	on: function() {}
}

var myObject = {};
Object.assign(myObject, EventTarget.prototype);

myObject.emit("somethingChanged");
```
而且需要注意的是，如果接收对象的属性时，多个对象的属性有同名的情况，则排名靠后的源对象属性会覆盖考前的源对象的属性

## 增强对象原型

原型是JavaScript继承的基础

### ES6中改变对象的原型

ES6中添加了Object.setPrototypeOf()方法来改变不能在对象实例化后改变原型的标准方法。
例如：
```javascript
//第一个参数是通过Object.create()创建的对象实例  第二个参数是要将该对象的原型改为哪一个对象原型
Object.setPrototypeOf(friend, dog);
```

## 简化原型访问的Super引用

ES5中实现重写对象实例的方法，以及调用和它同名的原型方法，较为麻烦.
例如：
```javascript
let dog = {
	getGreeting() {
	   return "WJC";
	}
};

let friend = {
	getGreeting() {
		return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
	}
};

//将原型设置为person
Object.setPrototypeOf(friend, person);
console.log(friend.getGreeting());
console.log(Object.getPrototypeOf(friend) == person);
即要通过Object.getPrototype(this).getGreeting.call(this)

ES6简化了访问原型 
let friend = {
	getGreeting() {
		return super.getGreeting() + ", hi!";
		//相当于ES5中 super.getGreeting()方法
	}
}
```