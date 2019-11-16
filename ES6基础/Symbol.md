在ES5即早期的语言中，包含5中原始类型:字符串型、数字型、布尔型、null和undefined，而ES6中引入了第6种原始类型：Symbol。

## 通过well-know Symbol暴露内部操作

ES5的一个中心主旨是将JavaScript中的一些"神奇"的部分暴露出来。
ES6延续了这个传统，新标准中主要通过在原型链上定义与Symbol相关的属性暴露更多的语言内部逻辑，并通过预定义一些well-konw Symbol来表示。
每一个这类Symbol都是Symbol对象的一个属性，例如Symbol.match。
这些well-know Symbol包括：
- Symbol.hasInstance  一个在执行instanceof时调用的内部方法，用于检测对象的继承信息。
- Symbol.isConcatSpreadable  一个布尔值，用于表示当传递一个集合作为Array.prototype.concat()方法的参数时，是否应该将集合内的元素规整到同一层级。
- Symbol.iterator 一个返回迭代器的方法。
- Symbol.match 一个调用String.prototype.match()方法时调用的方法，用于比较字符串。
- Symbol.replace 一个在调用String.prototype.replace()方法时调用的方法，用在字符串中定位子串。
- Symbol.search 一个在调用String.prototype.search()方法时调用的方法，用于分割字符串。
- Symbol.species 用于创建派生对象的构造函数
- Symbol.split 一个在调用String.prototype.split()方法时调用的方法，用于分割字符串
- Symbol.toPrimitive 一个返回对象原始值的方法
- Symbol.toStringTag 一个在调用Obejct.prototype.toString() 方法时使用的字符串，用于创建对象描述
- Symbol.unscopables 一个定义了一些不可被with语句引用的对象属性名称的对象结合

## Symbol.hasInstance方法
	
Symbol.hasInstance方法用于确定对象是否为函数的实例。Symbol.hasInstance被定义为不可写、不可配置并且不可枚举。	

## Symbol.isConcatSpreadable属性
	
Symbol.isConcatSpreadable属性是一个布尔值，如果该属性值为true，则表示对象有length属性和数字键，即它的数值类型属性值应该被独立添加到concat()调用的结果中。
例如：
```javascript
//定义一个类数组 设置其Symbol.isConcatSpreadable为true
let collection = {
	0: "hello",
	1: "world",
	length:2,
	[Symbol.isConcatSpreadable]: false
}
let messages = ["hi"].concat(collection);
//输出["hello, "world", "hi"]
```
注：如果Symbol.isConcatSpreadable属性设置为false，则在调用concat()方法时，元素不会被分解，返回结果会变成
```javascript
[ 'hi',
  { '0': 'hello',
    '1': 'world',
    length: 2,
    [Symbol(Symbol.isConcatSpreadable)]: false } ]
```

## Symbol.match、Symbol.replace、Symbol.search和Symbol.split属性

ES6定义了原生JavaScript中的match、replace、search、split方法对于的4个Symbol，将语言内建的RegExp对象的原生特性完全外包出来。

## Symbol.toPrimitive方法

ES6中通过Symbol.toPrimitive方法更改例如对象与字符串之间进行比较时转化的原始值类型。
Symbol.toPrimitive返回的分别时:数字、字符串、无类型的偏好值
对于大多数标准对象，数字模式有以下特性：
1. 调用valueOf()方法，如果为原始值，则返回	
2. 否则，调用toString()方法，如果解构为原始值，则返回
3. 如果再无可选值，则抛出错误。

对大多数标准对象，字符串模式有以下特性：
1. 调用toString()方法，如果结果为原始值，则返回
2. 否则，调用valueOf()方法，如果结果为原始值，则返回。
3. 如果再无可选值，则抛出错误。

默认模式只用于==运算、+运算及给Date构造函数传递一个参数时。在大多数的操作中，使用的都是字符串模式或数字模式