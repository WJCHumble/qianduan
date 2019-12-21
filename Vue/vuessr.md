在讲 Vue SSR 之前，先谈谈传统的 Vue 项目，传统的 Vue 项目只是一个单页应用，服务端给客户端返回的只有一个 HTML 和一堆 JS 文件，所以在做一些大型的网站，但是需要 SEO 的时候，传统的单页应用并不能提供这样的功能。因此，Vue 社区也针对这一问题，提出了 Vue SSR，并加以实践。

那么 Vue SSR 的优点是什么？

1. 更好地 SEO
2. 提高首屏到达速度
3. 解决了一些需要进行同步操作的场景

## 构建配置

一个完整的 Vue SSR 项目，它整体的构建过程是这样的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191219150504676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDQ5NDQ1,size_16,color_FFFFFF,t_70)

## 项目结构

一个简单的 Vue SSR 项目的结构：
```javascript
build
|—— webpack.client.config.js # 用于服务端的打包
|—— webpack.server.config.js # 用于客户端的打包
src
|—— store
    |—— index.js # 不同于传统的，它是一个工厂函数
|—— routes
    |—— index.js # 不同于传统的，它是一个工厂函数
├── components
│   ├── Foo.vue
│   ├── Bar.vue
│   └── Baz.vue
├── App.vue
├── app.js # 通用 entry(universal entry)，不同于传统的，它是一个工厂函数
├── entry-client.js # 仅运行于客户端（浏览器）
└── entry-server.js # 仅运行于服务器
server.js # 项目的入口文件
```
PS：这边我之前找了一个很简单的[Demo](https://github.com/tomashi/Vue2.0-SSR) ，有兴趣的可以去了解一下，不过深入学习还是推荐直接看官方给的 [Demo](https://github.com/vuejs/vue-hackernews-2.0/) 。

## 核心代码讲解

1.项目的入口文件 server.js

```javascript
/* server.js */
const express = require('express')
const app = express()
const renderer = require('vue-server-renderer').createRenderer()
const createApp = require('./dist/bundle.server.js')['default']

// 设置静态文件目录
app.use('/', exp.static(__dirname + '/dist'))

// 客户端打包地址
const clientBundleFileUrl = '/bundle.client.js'

// getHomeInfo请求
app.get('/api/getHomeInfo', (req, res) => {
    res.send('SSR发送请求')
})

// 响应路由请求
app.get('*', (req, res) => {
    const context = { url: req.url }

    // 创建vue实例，传入请求路由信息
    createApp(context).then(app => {
        let state = JSON.stringify(context.state)

        renderer.renderToString(app, (err, html) => {
            if (err) { return res.state(500).end('运行时错误') }
            res.send(`
                <!DOCTYPE html>
                <html lang="en">
                    <head>
                        <meta charset="UTF-8">
                        <title>Vue2.0 SSR渲染页面</title>
                        <script>window.__INITIAL_STATE__ = ${state}</script>
                        <script src="${clientBundleFileUrl}"></script>
                    </head>
                    <body>
                        <div id="app">${html}</div>
                    </body>
                </html>
            `)
        })
    }, err => {
        if(err.code === 404) { res.status(404).end('所请求的页面不存在') }
    })
})

// 服务器监听地址
app.listen(8080, () => {
    console.log('服务器已启动！')
})
```

2.用于创建Vue实例的 app.js

```javascript
/* main.js */
import Vue from 'vue'
import createRouter from './route.js'
import App from './App.vue'
import createStore from './store'


// 导出一个工厂函数，用于创建新的vue实例
export function createApp() {
    const router = createRouter()
    const store = createStore()
    const app = new Vue({
        router,
        store,
        render: h => h(App)
    })

    return { app, route, store }
}
```

3.服务端入口文件 entry-server.js

```javascript
/* entry-server.js */
import { createApp } from '../src/app'

export default context => {
    // 因为有可能会是异步路由钩子函数或组件，所以我们将返回一个 Promise，
    // 以便服务器能够等待所有的内容在渲染前，
    // 就已经准备就绪。
    return new Promise((resolve, reject) => {
        const {app, route, store} = createApp()

        router.push(context.url)

        // 获取相应路由下的组件
        const matchedComponents = router.getMatchedComponents()

        // 如果没有组件，说明该路由不存在，报错404
        if (!matchedComponents.length) { return reject({ code: 404 }) }

        // 遍历路由下所以的组件，如果有需要服务端渲染的请求，则进行请求
        Promise.all(matchedComponents.map(component => {
            if (component.asyncData) {
                return component.asyncData(store)
            }
        })).then(() => {
            context.state = store.state
            resolve(app)
        }).catch(reject)
    })

}
```

4.客户端入口文件 entry-client.js

```javascript
/* entry-client.js */
import { createApp } from '../src/app'


const {app, store} = createApp()

// 同步服务端信息
if (window.__INITIAL_STATE__) {
      store.replaceState(window.__INITIAL_STATE__)
}


// 绑定app根元素
window.onload = function() {
       app.$mount('#app')
}
```