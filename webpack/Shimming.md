# shimming 垫片

## 使用第三方模块

即我们可以通过 shimming 来实现对一些第三方模块的引入，例如配置 `jquery` 为 `$`

```javascript
{
	plugins: [
		new webpack.ProvidePlugin({
			$: "jquery"
		})
	]
}
```

即当存在模块中使用 `$` 时，会自动往该模块中引入 `jquery`，并命名该模块为 `$`。


## 使用第三方模块的某一个方法

例如使用 `loadash` 中的 `join` 方法：
```javascript
_join()
```

配置：
```javascript
{
	plugins: [
		new webpack.ProvidePlugin({
			_join: ['loadash', 'join']
		})
	]
}
```

## imports loader 改变 this 指向

通常情况下，模块中的 `this` 是指向其本身，如果我们需要改变 this 指向，则可以通过 `imports loader` 实现。

安装：
```javascript
npm i loadash-loader -D
```

配置：
```javascript
{
	module: {
		rules: [{
			test: /\.js$/,
			exclude: /node_modules/,
			use: [{
				loader: "babel-loader"
			},{
				loader: "imports-loader?this=>window"
			}]
		}]
	}
}
```

