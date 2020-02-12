## Resolve

Webpack 启动后会从配置的入口模块出发找所有的模块，Resolve 配置 Webpack 如何寻找模块所对应的文件。

### alias

alias 通过别名的方式定义一个路径的编写方式
```javascript
resolve: {
	alias: {
		_c: '../src/components'
	}
}
```

则项目中 
```javascript
import Modal from '_c/Modal'
```

实际上就等于
```javascript
import Modal from '../src/components/Modal'
```

alias 还可以通过 $ 声明，以什么关键字结尾的路径替换
```javascript
resolve: {
	alias: {
		'vue$': 'vue/dist/vue.esm.js'
	}
}
```

上面的 alias 会命中以 vue 结尾的 Import 语句
即
```javascript
import Vue from 'vue'
```

等价于
```javascript
import vue from 'vue/dist/vue.esm.js'
```
PS: 这就是经典的找 Vue 编译的源码的方法

### mainFields


### extensions

extensions 用于 webpack 在查找文件时，当导入的文件没有后缀时，优先查找的文件
例如，Vue CLI 项目中经典的配置：
```javascript
resolve: {
	extensions: ['.js', '.vue', '.json']
}
```
即此时，webpack 针对不带后缀的文件 a，会先找 a.ja，再找 a.vue，再找 a.json，并且需要注意的是如果都找不到会报错。

### modules

modules 用于配置 webpack 去哪些模块查找项目依赖的模块，例如使用 iview、ElementUI 等第三方库的时候，可能需要
```javascript
import { Button } from 'vant'
```

然后 webpack 则会默认会去 node_modules 下寻找 vant。但是如果我们存在自己项目中的依赖，这个时候我们可以用 alias 去定义一个依赖的公共路径，但是我们也可以通过 modules 定义 webpack 默认查找的文件目录
例如：
```javascript
modules: ['./src/components', 'node_modules']
```

### descriptionFiles

descriptionFiles 用于描述第三方模块的文件名
```javascript
descriptionFiles: ['package.json']
```

### enforceExtension

enforceExtension 顾名思义，即强制扩展名，如果为 true 则代表所有的导入语句都需要写入扩展名，默认是不需要开启的。

### enforceModuleExtension

enforceModuleExtension 和 enforceExtension 类似，但是它只对 node_modules 下的模块生效，即如果声明 enforceModuleExtension: false 那么第三方模块导入也得写扩展名