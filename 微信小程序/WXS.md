## WXS模块

WXS代码可以编写在wxml文件中的<wxs>标签，或以.wxs为后缀名的文件内。（**可以理解成网页中的Script标签**）

### 模块

每一个.wxs文件和<wxs>标签都是一个单独的模块
	每个模块都有自己独立的作用域，即在一个模块里面定义的变量与函数，默认为私有（对其他模块不可见）.
	一个模块要想对外暴露其内部的是由变量和函数，只能通过module.exports
	
##### 单独定义.wxs文件

即在.wxs文件中
```javascript
	module.exports = {}
```
	    
1. .wxml文件使用
	    			
```javascript
    <wxs src=".wxs文件相对路径" module="接受暴露的对象的变量"/>
    <view>{{变量名.属性}}</view>
```
 2. .wxs文件引用其他.wxs文件
	    			
```javascript
    var a = require('./a.wxs');
    var fn = a.fn;
    module.exports.fn = fn;
```

##### 在.wxml中使用wxs标签，直接写在.wxs中（此时是双标签）

例如： 
```javascript
	<wxs module="test3">
		var truth = "hello world";
		module.exports.truth = truth;
	</wxs>
	<view>{{test3.truth}}</view>
```

### 变量

wxs中变量的定义和JavaSript一样，诸如变量提升、未声明使用则未全局变量。
命名规则：
- 首字符必须是：字母（a-zA-Z），下划线（_）
- 剩余字符可以是：字母（a-zA-Z），下划线（_）， 数字（0-9）


保留关键字：
```javascript
	delete
	void 
	typeof
	null
	undefined
	NaN
	Infinity
	var
	if
	else
	true
	false
	require
	this
	function
	arguments
	return
	for
	while
	do
	break
	continue
	switch
	case
	default
```

### 数据类型

wxs共有8种数据类型：

```javascript
	number        
	string
	boolean
	object
	function
	array
	date
	regexp
```
相应的方法也是遵循ES5中定义的，此处不一一列举。

**需要注意：**

```javascript
 date是通过getDate()函数生成，并不是实例Date对象，getDate获取的对象也同样有getFullYear、getMonth等方法
 regexp是通过getRegxp(pattern[, flags])
```

参数：   
- pattern: 正则表达式的内容。
- flags：修饰符。该字段只能包含以下字符:
- g: global
- i: ignoreCase
- m: multiline。

方法：  
- test
- exec
也可以先创建正则及要匹配的表达式内容，调用global()、ignoreCase()、multiline方法进行

#### 数据类型判断constructor

例如: 
			
```javascript
    var a = 100;
  	console.log("Number" === a.constructor)
```
