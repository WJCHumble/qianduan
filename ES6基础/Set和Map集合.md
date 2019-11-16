
## ES5中的Set集合和Map集合

ES5中常用Object的属性来模拟创建Set和Map集合
注：这种方式会存在诸多缺陷例如
```javascript
var map = Object.create(null);
map[5] = 'foo';

console.log(map['5']);
//即会map[5] 和 map['5']访问的属性是一样的
```

## ES6中的Set集合
	
通过调用new Set()创建Set集合，实例一个Set集合对象。
Set集合对象属性：size  获取集合中元素的个数

Set集合对象方法：
		add()  向集合中添加元素
		has()  检测集合中是否存在某个元素
		delete()  移除指定的元素
		foEach(value, key, ownerSet)  使用方法和数组的forEach()类似，不过Set集合中的三个参数不同与数组的forEach
				                      - value     元素值
				                      - key       元素的索引
				                      - ownerSet  被遍历的Set集合本身

ES6中的Set结合对象的实例，每一个属性都是独立的，即不会上面所述ES5中存在的情况。并且Seet结合会过滤掉重复传入的元素（即自动去重）

注：Set集合不能像数组那样通过索引来访问集合中的元素，如果需要，要把Set集合转换成一个数组
	
### Set集合转换为数组

将数组转化为Set集合，直接通过将数组传入Set函数的构造函数即可。
例如：
```javascript
let set = new Set([1, 2, 3, 4]);
```

将Set集合转换为数组，只要通过[]的方式创建数组，并且通过...展开运算符将set集合传入[]中即可
例如:
```javascript
let set = new Set([1, 2]),
		array = [...set];
```
前面以及提过Set集合可以去重，那么可以借助Set集合处理数组，进行去重。

### Weak Set集合

在Set结合中添加元素，只能去除元素本身的引但是不能去除Set集合保留的引用。而Weak Set集合则可以解决这种情况。
WeakSet对象的方法：
- add()
- has()
- delete()   可以删除集合中指定的元素，并且去除集合中的引用
使用WeakSet集合对象需要注意：
1. add()添加非对象参数会导致程序报错
2. has()和delete()方法传入非对象参数始终返回false
3. 不可迭代
4. 不支持size属性

## ES6中的Map集合

Map是一种存储键值对的形式存储元素的有序列表，其中键名和对应的值支持所有数据类型。（键名的等价性可以通过Object.is()方法进行判断）

补充：Map与对象不同，对象的属性名总是会强制转换成字符串类型，但是Map中数字5和'5'是两种类型。

Map集合对象的属性：size   返回集合的长度
Map集合对象的方法：
- set(key, value)
- get(key)
- has(key)
- delete(key)
- clear()   清除Map集合中的所有键值对

### Map集合的初始化方法

类似于Set集合的构造函数初始化的方式，可以向Map的构造函数中传数组的方式初始化Map集合，不过由于Map集合中的元素是以键值对的形式，所以传入数组必须是二维的数组，并且一个键值对对应二维中的一个数组。
例如：	
```javascript
let set = new Set([1, 2]),
		array = [...set];
```

#### Map集合的forEach()方法

和Set集合一样，不再做描述。	

#### Weak Map集合

类似于WeaKSet集合，也是一种弱引用的Set集合，即可移除已加入元素的引用，并且添加元素的键名必须是一个非空对象，否则会报错。

用途：用于保存web页面中的DOM元素，例如一些JavaScript库就是通过自定义的对象保存每一个引用的DOM

当移除其他元素对改对象的引用以及设置该对象等于null WeakMap集合会自定移除对它的引用

WeakMap集合对象的方法：
- has()
- get()
- set()
- delete()

##### Weak Map集合的初始化
	
```javascript
	let key1 = {},
	key2 = {},
	map = new WeakMap([[key1, 'wjc'], [key2, 21]]);	
```	

注：每一个元素的键名必须是对象