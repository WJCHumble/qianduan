## Module

webpack 中 Module 规定了如何处理模块

### 配置 Loader

配置 rules 的步骤：
1. 条件匹配，即通过 test、include、exclude 三个配置来对指定的文件匹配对应的 Loader。
2. 应用规则，即通过 use 来使用 Loader，也可以只应用一个 Loader，也可以是多个 Loader。并且也可以给 Loader 通过 querying 的形式传参数。
3. 重置顺序，多个 Loader 的执行顺序默认是从右到左执行，但是通过 enforce 选项可以让其中一个 Loader 的执行顺序放到最前或者最后。
例如：
```javascript
module: {
	rules: [
		{
			test: /\.js$/,
			// 使用 babel-loader 转换 javascript 文件，用于缓存 babel 编译结果加快重新编译速度
			use: ['babel-loader?cacheDirectory'],
			// 只命中 src 目录里的 js 文件，加快 webpack 搜索速度
			include: path.resolve(__dirname, 'src')
		},
		{
			test: /\.scss$/,
			// 此时就需要一组 loader 去处理 scss 文件
			use: ['sytle-loader', 'css-loader', 'sass-loader'],
			// 排除 node_modules 目录下的文件
			exclude: path.resolve(__dirname, 'node_modules')
		},
		{
			// 对非文本文件采用 file-loader 加载
			test: /\.(gif|png|jpe?g|eot|woff|ttf|svg|pdf)$/i,
			use: ['file-loader']
		}
	]
}
```

当一个 loader 需要传多个参数的时候，使用 query 不太友好，这个时候可以使用 options 对象的形式指定多个参数。
```javascript
use: [
	{
		loader: 'babel-loader',
		options: {
			cacheDirectory: true,
		},
		// enforce 有两个值 post 让 loader 放到最后执行，pre则到最前面执行
		enforce: 'post'
	},
]
```

其实对于 test、include、exclude 还支持匹配多个条件（每个条件间是或），这个时候则需要用数组的形式传入
```javascript
{
	test: [
		/\.jsx?$/,
		/\.tsx?$/
	],
	include: [
		path.resolve(__dirname, 'src'),
		path.resolve(__dirname, 'tests')
	],
	exclude: [
		path.resolve(__dirname, 'node_modules'),
		path.resolve(__dirname, 'bower_modules')
	]
}
```

#### noParse

noParse 可以让 Webpack 在构建时对没采用模块化的文件的解析，从而提高构建性能。
noParse 对应的值是 RegExp、[RegExp]、function 其中一个
```javascript
noParse: /jquery|chartjs/
```

#### parse

Webpack 支持 AMD、CommonJS、SystemJS、ES6。而，与 noParse 不同的是，parse 可以控制是否的场景
```javascript
module: {
	rules: [
		{
			test: /\.js$/,
			use: ['babel-loader'],
			parse: {
				amd: false,
				commmonjs: false,
				system: false,
				harmony: false,
				requrieInclude: false,
				requireEnsure: false,
				requireContext: false,
				broserify: false,
				requireJs: false
			}
		}
	]
}
```