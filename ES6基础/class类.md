## 类的主体
	
### 属性
	
ES6中任然保留prototype，即可以直接在类中添加定义方法，用于覆盖/初始化添加方法

### 静态属性
	
以static关键字修饰静态属性，和其他语言类似，静态属性需要通过类名访问
例如： 
```javascript
class Example {
			static a = 2	
		}
//访问静态属性a
Example.a
```

### 公共属性

即通过prototype定义的属性。
例如： 
```javascript
class Example{}
Example.prototype.a = 2;
```

### 实例属性

指定义在实例对象this上的属性
```javascript
class Example {
	a = 2;
	constructor () {
		console.log(this.a);
	}
}

```
### name属性

和JavaScript一样的通过name属性可以访问类名（方法名）

### 方法
	
构造方法，即constructor方法，类的默认方法，创建类的实例对象时自动调用
例如：
```javascript
class Example {
	constructor() {
		console.log("我是constuctor");
	}
}
new Example(); //输出我是constructor
```

## 类的实例化

类的实例化必须通过new关键字
实例化的对象都共享一个原型对象，即实例对象1._proto_ == 实例对象2._proto_，会返回true

## 方法修饰

暂时不拓展

## 封装与继承

### getter/setter

需要注意的几点:
- getter、setter不能单独出现
- getter、setter必须同级出现，即要么全放在父类中，要么全放在子类中，不能一个子类一个父类

### extends继承

通过extends实现类的继承
例如
```javascript
class Child extends Father {}
```

### super

通过继承，子类中的构造方法constructor中必须要有super()方法，并且需要出现this之前。
调用父类的普通方法通过super.方法名()
调用父类的静态方法通过super.静态方法名