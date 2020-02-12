# 配置

## Entry

webpack 进行构建的开始就是 entry 文件开始，递归解析出所有入口依赖的模块。

### context

webpack 构建时寻找相对路径时会以 context 为根目录。而默认的 context 当前 webpack 启动所在的目录。我们也可以手动更改 context，例如：
```javascript
module.exports = {
	context: path.resolve(__dirname, 'test')
}
```

此外，还可以通过 shell 命令参数的形式设置 context，即 webpack --context

Entry 类型：
- string，例如 "./src/entry.js"
- array，相当于 string 类型数组，例如 ["./src/a.js", "./src/b.js"]
- object，配置多个入口，且每一个入口生成一个 chunk，{a: './src/a.js', b: './src/b,js'}

### Chunk 名称 

webpack 会为每个生成的 chunk 取一个名称，chunk 的名称和 entry 的配置相关。
- 当 entry 为 string 或 array，只会生成一个 chunk，此时 chunk 名称为 main
- 当 entry 为 object，会生成多个 chunk，此时 chunk 名称为 object 中的键名

## Output

在 webpack 中 output 定义了我们如何输出打包后的代码。

### filename

filename 则用于设置输出文件名称，为 string 类型。如果项目中只有一个输出文件，则可以直接写成静态的。
```javascript
filename: 'bundle.js'
```

如果项目中存在多个 chunk 的时候，我们可以根据 chunk 去设置输出文件名称。
```javascript
filename: '[name].js'
```
这里的意思就是用 webpack 内置的 name（即 chunk 名称）来代替 [name]，最终用来定义输出文件的名称。
内置的变量还有：
变量名     |  含义
-------- | -----
id  | chunk 的唯一标识，从 0 开始
name  | chunk 的名称
hash  | chunk 的唯一标识的 Hash 值
chunkhash  | chunk 内容的 Hash 值

在使用 hash 或 chunkhash 的时候，我们也可以指定它们的长度，例如 [hash:8] 则指定 chunk 的唯一标识长度为 8 的 hash 值。

### chunkFilename

略

### path

path 是用于配置打包后输出文件存放的位置，通常是通过 Node.js 的 path 模块去获取绝对路径。
```javascript
path: path.resolve(__dirname, './dist')
```

### publicPath

通常在项目打包上线时，一般会将打包生成的 JS 文件放到 CDN 上面，这时候就需要配置 publicPath，使得打包后引入的 JS 的路径是指向 CDN 上的资源。
例如：
```javascript
output : {
	filename: 'bundle.js',
	publicPath: 'https://cdn.wujingchang.com/asstes'
}
```
则打包输出的 HTML 代码中对 JS 的引入会是这样的：
```javascript
<script src="https://cdn.wujingchang.com/asstes/bundle.js"></script>
```

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

