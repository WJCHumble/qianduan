## Plugin

在 webpack 中 plugin 用于扩展 webpack 的功能，例如生成 html 文件、压缩 css 文件。

### 配置 Plugin

在 webpack 中配置 plugins 只需要向该数组中传入相应的 plugin 实例
```javascript
const CommonsChunkPlugin = require('webpack/lib/optimize/CommonsPlugin')

module.exports = {
	plugins: [
		new CommonsChunkPlugin({
			name: 'common',
			chunks: ['a', 'b']
		})
	]
}
```