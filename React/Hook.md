# Hook

Hook的意义就是让函数组件也能使用state，以及进行一些state的操作	

## State Hook

State Hook在函数组件中的具体表现形式是通过使用userState Hook
例如：
```javascript
	import React, { useState } from 'react'

	function Example() {
		// 此时相当于定义了一个state变量名为 count
		// 定义了一个修改state的方法setCount
		const [count, setCount] = useState(0)

		return (
			<p>You clicked {count} times</p>
			<button onClick={() => setCount(count + 1)}>
				Click me
			</button>
		)
	}
```

### 调用state

和class组件不同的是，在函数组件中调用state可以直接写函数名
例如使用state中的名为count的变量：
1. class组件中：

```javascript
	{this.state.count}
```

2. 函数组件中：

```javascript
	{count}
```

### 更新state

1. class组件中：

```javascript
	<p onClick={() => this.setState({count: this.state.count + 1})}></p>
```
2.  函数组件中，则可以直接使用useState中定义的第二个参数，即是对应修改state的方法:

```javascript
	<p onClick={() => setCount(count + 1)}></p>
```

## Effect Hook

Effect Hook可以在函数组件中执行副作用操作

### 副作用操作

在React中，数据获取、设置订阅、手动更改React组件中的DOM都属于副作用操作。（即可以将useEffect Hook看作**componentDidMount**、**componentDidUpdate**、**componentWillMount**三个生命周期函数的**组成**）

在React中两种常见副作用操作：**需要清除**和**不需要清除的**

#### 无需清除的effect

例如在更新DOM之后运行一些额外的代码。例如发网络请求、手动变更DOM、记录日志等等

1. 在class组件中的实现
一般把副作用操作放到componentDidMount和componentDidUpdate函数中
例如：

```javascript
	componentDidMount() {
		console.log(111)
	}
	componentDidUpdate() {
		console.log(111)
	}
```

这两个生命周期函数实现的效果：会在组件挂载、和组件更新的时候会输出111
2. 在函数组件中的实现

```javascript
	useEffect(() => {
		console.log(111)	
	})		
```

useEffect()会实现上面函数组件实现的效果，即useEffect()会在第一次渲染和每次更新后都会执行

#### 需要清除的effect

例如订阅外部数据源这类的副作用就得清除	
1.  在class组件中实现(以React官网的例子举例)

一般会在componentDidMount中设置订阅，在componentWillUnmount中清除它
假设有一个ChatAPI模块，它允许我们订阅好友的在线状态

```javascript
	componentDidMount() {
		ChatAPI.subscribeToFirendStatus(
			this.props.friend.id,
			this.handleStatusChange
		)
	}
	componentWillUnmount() {
		ChatAPI.unsubscribeFromFirendStatus(
			this.props.friend.id,
			this.handleStatusChange
		)
	}
	handleStatusChange(status) {
		thsi.setState({
			isOnline: status.isOnline	
		})
	}
```

2. 在函数组件中实现

```javascript
	useEffect(() => {
		function handleStatusChange(status) {
			setIsOnline(status, isOnline)				
		}

		ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

		return function cleanup() {
			ChatAPI.unsubscribeFromFriendStatus(prosp.friend.id, handleStatusChange)
		}
	})
```


