> **前言**

学过Vue的都知道，Vue的全局状态管理是由Vuex完成的。同样的React也有属于它的全局的状态管理，最著名的就是Flux和Redux，并且作为Flux之后出现的Redux，完美的接收了Flux的优点，并很好的弥补了Flux的不足。而Redux则由：Action、Reducer、Store组成。

### Action

在[Redux的官方文档](http://cn.redux.js.org/docs/basics/Actions.html)是这样描述的：Action是把数据从应用传到store的有效载荷，是store数据的唯一来源，可以通过store.dispatch()将action传到store。**简单地理解就是**Action是用来描述要==操作store的动作标识==(即描述发生的事情，并没有进行)，Reducer是用于识别特定的动作标识作出特定的状态的变化 (即store的数据变化)
****
**基本语法**

```javascript
	//定义action类型
	const action_type = 'action_type'

	//action创建函数
	export function doSomething(index) {
		return {type: action_type, index}
	}
```

### Reducer

Reducers则指定了应用状态（Store）的变化，以及响应actions并发送到store，Reducer是专门对state进行计算，返回下一个state。(需要注意的是，在Reducer中只能进行对store数据的变化的操作，不能操作API、或者进行异步操作)
****
**基础语法**

```javascript
	import {combineReducers} from 'redux'
	import action_type from './action.js'

	//定义初始的state值
	const initialState = {
		attrName: 'attrValue'
	}
	
	//创建reducer函数
	function reducerFn(state = initialState, action) {
		switch(action.type) {
			case 'action_type': 
				return Object.assign({}, state, {
					action.action.id
				});
				break;
			default:
				return state;  //必须返回一个state
				break;
		}
	}

	//如果存在多个reducer函数，可以使用Redux提供的combineReducers结合一起
	export defautl combineReducers({
		reducerFn,
		......
	})
```

### Store

在Redux中Store就是将Reducer、Action联系起来的作用.
**Store的职责：**
- 维持应用的state
- 提供getState()方法获取state
- 提供dispatch(action)方法更新state
- 通过subscribe(listener)注册监听器
- 通过unsubscribe(listener)返回的函数注销监听器
****
**基本语法**

```javascript
	import {createStore} from 'redux'	
	import reducerFns from './reducers'
	
	let store = createStore(reducerFns)
```
