#### 数据绑定

wxml中：
```javascript
	<view>{{message}}</view>	
```

page.js中：
```javascript
	Page({
		data: {
			message: 'hello WJC'
		}
	})
```

#### 列表渲染


wxml中:
```javascript
	<view wx:for="{{array}}">{{item}}</view>
```

page.js中：
```javascript
	Page({
		data: {
			array: [1, 2, 3, 4, 5]		
		}
	})
```

#### 条件渲染

wxml中:
```javascript
	<view wx:if="{{view == 'WJC'}}">welcome to WJC</view>
```

Page.js中：
```javascript
	Page({
		data: {
			view: 'WJC'
		}
	})
```

#### 模板

wxml中：
```javascript
	//定义一个模板
	<template name="stafName">
	    <view>
			FirstName: {{firstName}, LastName: {{lastName}}}
		</view>
	</template>
	//使用该模板
	//其中data属性用于传入一个对象，改对象中的属性是模板中需要的
	//注意这个对象一定要用{...}展开
	<template is="stafName" data="{{...obj}}"></template>
```

Page.js中：
```javascript
	Page({
		data: {
			obj: {
				firstName: 'wjc',
				lastName: 'wjc2'
			}
		}
	})
```

#### 引用模板

##### import引用

单独定义一个item.wxml
```javascript
	//定义一个模板
	<template name="item">
		<view>{{text}}</view>
	</template>
```

在index.wxml中引用
```javascript
	//import引入该模板文件
	<import src="item.wxml"/>
	//使用改模板
	<template name="item" data="{{text: 'hello world'}}"></template>
```

##### include引用

include使用与import相同，与import不同的是它是将引用的wxml文件中所有全部引用到这，而不仅仅是template模板（等同于php中的include）。
