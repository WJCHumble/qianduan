## 事件简写

### 1. 事件

#### 1.1 事件简写

v-on:click=''
简写方式 @click=''

#### 1.2 事件对象$event

包含事件相关信息,如事件源target、事件类型type....

#### 1.3 事件冒泡

阻止事件冒泡
    a)原生js,依赖事件对象 e.stopPropagation()
    b)vue方式 @click.stop

#### 1.4 事件默认行为

阻止默认行为例如a链接之类的
    a)原生js,依赖事件对象 e.preventDefault()
    b)vue方式 @click.prevent

#### 1.5 键盘事件

回车: @key.13 或 @keydown.enter 
上: @key.38 或 @keydown.up 

默认没有@keydown.a/b/c 可以自定义键位别名
Vue.config.keyCodes = {
    a:65,
    f1:112,
}

#### 1.6 事件修饰符

.stop 调用event.stopPropagation()
.prevent 调用event.preventDefault()
.{keyCode | keyAlias} 只当事件从特定键触发时才触发回调
.native 监听组件根元素的原生事件
.once 只触发一次回调

### 2. 属性

#### 2.1 属性的绑定和属性的简写

v-bind 用于属性绑定 v-bind:属性=""

属性绑定的简写:
    v-bind:src=''简写为:src=""

#### 2.2 class属性和style属性

绑定class和style属性时语法比较复杂

## 六、模板

### 1.简介

Vue.js使用基于HTML的模板语法将DOM绑定到Vue实例中的数据
模板就是{{}} 用来进行数据绑定 显示在页面中
{{}}也称为Mustache语法

### 2.数据绑定的方式

a.双向绑定
    v-model
b.单向绑定
    方式1:使用双对大括号{{}},可能会出现闪烁的问题,可以使用v-cloak
    方式2:使用v-bind

### 3.其他指令

v-once 数据只绑定一次 直接使用v-once不用=什么
v-pre  不编译 直接原样显示
    

## 七、过滤器

### 1.简介

用来过滤模型数据，在显示之前进行数处理和筛选
语法:{{data | filter(参数)|}}
    
### 2.内置过滤器

vue2.0中已删除了所有内置过滤器，全部被废除
解决方式:
        a.使用第三方工具库，如loadash(下划线库)、date-fns日期格式化、accounting.js货币格式化
        b.可以自定义过滤器

### 3.自定义过滤器

分类:全局过滤器、局部过滤器 
全局:
```javascript
Vue.filter('name', function(){})
``

局部:
```javascript
var vm = new Vue({
       el:'#app',
       data:{},
       filters:{
                
            }
       })
```


## 八、发送ajax请求

### 1.通过第三方包axios发送

axios.get(url[, config])
传参方式:
    1.通过url传参
    2.通过params选项传参
    3.如果使用模块化开发，可以使用qs模块进行转换
    注:axios不支持跨域
    axios.post(url[, data[, config]])

axios默认发送数据时,是post请求时,数据格式是Request Payload(即默认是传的json),并非我们常见的Form Data,所以参数必须要以键值对的形式传递
解决方法:自己拼接为键值对
```javascript
axios.post('index.php','name=wjc&age=21')
        .then(function(response){
            // console.log(response);
            document.write(response.data);
        })
```
当传递数据为变量的时候:
```javascript
axios.post('index.php',this.user,{
    transformRequest:[
        function(data){
            let params = '';
            for(let index in data){
                params += index + '=' + data[index] + '&';
            }
            console.log(params);
            return params;
        }
    ]
})
.then(function(response){
    // console.log(response);
    document.write(response.data);
})
```
    transformRequest即在传递变量的前对数据进行改造
注：建议还是用vue.resource好一些....

### 2.通过vue.resource.js发送

get请求:
```javascript
this.$http.get(url, {params:JSON}).then(function(res){
            //res.body是返回结果
            },function(res){
            //status返回状态码
        })
```

post请求:
```javascript
this.$http.post(url, JSON, {emulateJSON:true}).then(funtion(
           res){
            
            },function(res){

         })
```

jsonp跨域请求:
```javascript
this.$http.jsonp(url, {{params:JSON},jsonp:'callback的名称'}).then(function(res){

                },function(res){

                })
```
如果要修改callback的名称则修改就行,不修改的话跨域不写那个参数.

## 九、Vue生命周期(总共有11个,此处只简单列举几个重要的)

### beforeCreate()
        创建前
### created()
        创建后
### beforeMount()
        挂载前
### mounted()
        挂载后
### beforeUpdate()
        更新前
### updated()
        更新后
### beforedestroy()
        销毁前
### destroyed()
        销毁后
        PS:此处的销毁指的是整个vue实例,销毁方法vm.$destroy()

## 十.Computed计算属性

### 1.计算属性也是用来存储数据,它具有以下特点:

a.数据可以进行逻辑处理操作
b.对计算属性中的数据进行监听

### 2.计算属性和方法的区别

方法也可以实现计算属性的功能
区别:
a.计算属性是基于它的依赖进行更新的,只有相关依赖发生改变的时候才能更新变化
b.计算属性是有缓存的,只要相关依赖没有改变,多次访问计算属性得到的值是之前缓存的结果,即只要计算属性所依赖的对象没发生变化,多次访问只会执行一次。  
PS:还需要注意的是计算属性调用vm.属性名  不要写括号!

### 3.get和set

计算属性由两部分组成:get和set 分别用于获取或设置计算属性
默认只有get,如果需要set,需要自己添加

## 十一、Vue实例的属性和方法

### 属性
    vm.$data              查看data选项
    vm.$props
    vm.$el                获取vue实例绑定的对象dom
    vm.$options           获取自定义的选项
    vm.$parent
    vm.$root
    vm.$scopedSlots
    vm.$refs              当要操作dom的时候，只需给该元素设置ref='name' 调用vm.$refs.name      则可获得需要操作的dom元素
    vm.$isServer          查看当前实例是否运行在服务器上
    vm.$attrs
    vm.$listeners

### 方法

vm.$mount()
vm.$destroy()
vm.$nextTick(callback)      由于Vue实现响应式,所以并不是数据更新dom也会随着立即更新,这个需要时间,所以如果需要获取更新过数据的dom需要使用该方法
vm.$set(obj, attr, val)    如果需要对对象添加属性后,页面监听更新该对象,则需要使用该方法
vm.$delete(object, key)
vm.$watch(data, callback[,option])                建议使用vue提供的watch选项,如
```javascript
var vm = new Vue({
    el:'#app',
    data:{
        msg:'WJCHumble',
        user:{
            name:'stunning',
            age:21
        }
    },
    //方法2
    watch:{
        msg(val, oval){
            console.log('新数据为:'+val+' '+'旧数据为:'+oval);
        },
        //监听对象
        user:{
            handler:function(val, oval){
                console.log('新数据为:'+val.name+' '+'旧数据是:'+oval.name);
            },
            deep:true
        }
    }
}); 
```
                               
PS:需要注意的是监听对象是需要以对象的形式进行监听,有两个参数很关键handler和deep

## 十二、自定义指令

### 全局指令

```javascript
Vue.directive('name',{
    //钩子函数
    bind(){//只调用一次,指令第一次绑定到元素时调用

        },
    inserted(){//被绑定元素插入父节点的时候调用

        },
    update(){//被绑定元素更新时调用

        },
    componentUpdated(){//指令所在节点以及它的子节点全部更新后调用

        },
    unbind(){//只绑定一次,指令与元素解绑时调用

    }
  })
```
        //钩子函数参数
        el  dom对象 
        binding 一个对象 它包含的属性:name 指令名但不包括v-前缀
                                      value指令绑定的值(如果传入的是计算表达式相应的value是计算的结果) 
                                      oldValue指令绑定前的值 仅在update和componentUpdated()钩子中有用
                                      expression 字符串式的指令表达式 
                                      arg传给指令的参数 如v-my-directive:fo 则参数为fo
                                      modifiers一个包含修饰符的对象 例如v-my-directive.foo.bar 则修饰符对象为{foo:true, bar:true}
        vnode Vue编译生成的虚拟节点
        oldNode 上一个虚拟节点
        PS:主要用你el 和 binding两个钩子函数

### 局部指令

    PS:与全局不同的地方只是定义在实例中,其他相同。


## 十三、动画效果

### 1.简介

    Vue 在插入、更新或移除DOM时提供多种不同的方式的应用过渡效果
    本质上还是使用css3动画:transition、animation

### 2.基本用法

    使用transition组件  将要执行的动画的元素包含在该组件内
    例:  <transition name=''><transition>  

### 3.钩子函数

    8个
    <transition
     @before-enter=''
     @enter=''
     @after-enter=''
     @before-leave=''
     @leave=''
     @after-leave=''
     >
    </transtion>

### 4.结合第三方动画库animate.css

    <link rel="stylesheet" href="animate.min.css" />
    <transition
     enter-active-class='animated fadeIn' 
     leave-active-class='animated fadeOut'
     >
     </transition>
     PS:animate的动画类需要结合animated一起使用

### 5.列表过渡

    <transition-group>

    </transition-group>
    PS:这样就可以实现多个元素过渡

## 十四、组件component

### 1. 什么是组件

组件(component)是Vue.js最强大的功能之一。组件可以扩展HTML元素封装可重用的代码，组件是自定义元素（对象）

### 2. 定义组件的方式

方式1：先创建组件构造器，然后由组件构造器创建组件
方式2：直接创建组件

### 3. 组件分类

分类：全局组件、局部组件

### 4. 引用模板

将组件内容放到模板<template>中并引用

### 5. 动态组件

<component :is='组件标签'>
当多个组件使用同一个挂载点，然后动态的在它们之间切换的时候可以使用

<keep-alive> 可以将未被切换的组件保留的缓存中，避免默认销毁未被切换组件，影响加载效率

## 十五、 组件间的数据传递

### 1. 父组件           

在一个组件内部定义另一个组件，称为父子组件
子组件只能在父组件内部使用
默认情况下，子组件无法访问父组件中的数据，每一个组件实现的作用域是独立的

### 2. 组件间数据传递(通信)    

#### 2.1 子组件访问父组件的数据

a)在调用子组件时，绑定想要获取的父组件中的数据
b)在子组件内部，使用props选项声明获取的数据，即接收来自父组件的数据
注:组件中的数据有三种形式:data、props、computed

PS:父组件向子组件传递数据,通过子组件在props中声明,然后在父组件中绑定变量值

#### 2.2 父组件访问子组件的数据

a)在子组件中使用vm.$emit(时间名,数据)触发一个自定义事件，事件名自定义
b)父组件在使用子组件的地方监听子组件触发的事件,并在父组件中定义方法,用来获取数据

### 3. 单向数据流

props是单向绑定的，当父组件的属性变化时，将传导给子组件，但是不会反过来。
解决方法：
      方法1:如果子组件想把它作为局部数据来使用，可以将数据存入另一个变量中再操作，不影响父组件中的数据
      方法2:如果子组件想修改数据并且同步更新到如组件，有两个方法:
            a.使用.sync(1.0版本支持，2.0版本不支持，2.3又支持)
              例：<my-hello :name.sync='name'></my-hello>
              修改数据需要这样: this.$emit('update:name', 'wjc');    格式this.$emit('update:foo', 'newValue')  foo是指修改的属性名
            b.在子组件中将父组件引用的数据包装成对象，然后再修改数据(这是在开发中常用的)

### 4. 非父子组件之间的通信

非父子组件之间的通信，可以通过一个空的vue(假设该实例的名称为evm)实例作为中央事件总线，然后通过发送和监听该对象完成非父子组件之间的数据的传输，如：
```javascript
evm.$emit('v-a',a) //组件a中发送事件
//如果组件b要接收,可以放在该组件的mounted挂载完的事件中执行
mounted(){
  //需要注意的是$on()的第二个参数是一个回调函数
  evm.$on('v-a',a=>{
    this.a = a
    })
}
```

## 十六、slot内容分发

本意：位置、槽
作用:可以在组件中写相应的html标签即嵌入内容

## 十七、vue-router路由

### 1. 简介

使用Vue.js开发SPA(Single Page Application) 单页面应用
根据不同的url地址，显示不同的内容，单显示在同一页面中，称为单页面应用。
基本步骤：
a.在html中输入     
```javascript
  <router-link to='/home'>首页</router-link>
  <router-link to='/about'>关于我们</router-link>

  <router-view></router-view>
```
PS:在浏览器运行的时候会发现vue提供了几个路由处于激活状态的类选择器，我们可以通过该类选择器来设置激活类选择器的样式，例如
```javascript
router-link-active{
  font-size:20px;
  color:#faa;
  text-decoration:none;
}
```

### 路由嵌套和参数传递

即在已有的路由的模板的基础上进行嵌套，例如:
```javascript
<!-- 这个是一个路由组件的内容 -->
<template id='user'>
   <div>
       <ul>
           <router-link to='/login?username=wjc&age=21' tag='li'>用户登录</router-link>
           <router-link to='/register/zfh/21' tag='li'>用户注册</router-link>
           <router-view></router-view>  <!-- router-view记得一定要写 -->
       </ul>
   </div> 
</template>
```
然后在创建该路由组件的时候写入children属性
```javascript
var routes = [
  {
      path:'/user', 
      component:user,
      children:[//路由嵌套，相当于孩子children
          {path:'/login', component:login},
          {path:'/register/:username/:age', component:register}
      ]
  }
]
```

PS:其实就相当于子路(children)与父路由之间的关系，子路由的组件内容(此处为login、register)也需要创建。
子路由中接组件参数：普通的搜索式的参数直接用{{$route.query}}
                     rest式的参数用{{$route.params}}

### 导航守卫

vue-router提供的导航守卫主要用来通过跳转或取消的方式取消守卫导航。有多种置入路由导航过程中：全局的，单个路由独享的，或者组件级的。

####  全局前置守卫

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫时异步解析执行，此时导航在所有守卫resolve完之前一直处于等待中。
每个守卫方法接收三个参数：
        to: Route  即将进入的目标路由对象
        from: Route  当前导航正要离开的路由
        next: Function  必须调用next方法resolve这个钩子，其执行效果依赖next方法调用的参数
            next()  进入管道的下一个钩子，如果全部不钩子执行完了，则导航的状态是confirm的
            next(false)  中断当前的导航，即不让用户进行跳转
            next('/')或者neext({path: '/'})跳转到一个不同的地址。当前导航被中断后，然后进行一个新的导航。可以像next传递任意位置对象，例如设置raplace: true name: 'home' 之类的选项以及如何用在router-link的to prop或router.push()的选项
        next(error):  如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。
注：确保要调用 next 方法，否则钩子就不会被 resolved。
基本语法：
```javascript
//全局前置守卫
//一定要调用next!!!
router.beforeEach((to, from, next) =>  {
  // console.log(to.path);
  // console.log(from.path);
  console.log("我是前置守卫");
  //false会中断路由，即无论怎么点击都是无效的!
  // next(false);
  if(to.path == "/profile"){
    next();
  }else {
    next(false);
  }
})
``

#### 全局解析守卫

在 2.5.0+ 你可以用 router.beforeResolve 注册一个全局守卫。这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。

基本语法：在导航被确认之前，同时所有组件内是守卫和异步路由组件被解析之后，解析守卫就会被调用
```javascript
router.beforeResolve((to, from, next) => {
  console.log("我是解析守卫");
  console.log(to.path);
  console.log(from.path);
  next({path: '/index', replace: true});
  console.log(next);
  next();
  //next({path: 'index', query: {toPath:'/index'}});
})
```
注：全局解析守卫是在全局前置守卫之后调用的

#### 全局后置钩子

全局后置钩子，和守卫不同的是，这些钩子不会接受 next 函数也不会改变导航本身：

基本语法：
```javascript
router.afterEach((to, from) => {
  console.log("我是全局后置");
});
```

注：全局后置钩子是在全局解析守卫之后执行

#### 路由独享的守卫

可以直接在路由配置上直接定义beforeEnter守卫:
```javascript
routes: [
  {
    path: '/foo',
    component: Foo,
    beforeEnter: (to, from, next)=>{
      //...
    }
  }
]
```

#### 组件内的守卫

可以直接在路由组件中定义以下路由导航守卫：
beforeRouteEnter
beforeRouteUpdate (2.2 新增)
beforeRouteLeave
 ```javascript
 const Foo = {
   template: `...`,
   beforeRouteEnter (to, from, next) {
     // 在渲染该组件的对应路由被 confirm 前调用
     // 不！能！获取组件实例 `this`
     // 因为当守卫执行前，组件实例还没被创建
   },
   beforeRouteUpdate (to, from, next) {
     // 在当前路由改变，但是该组件被复用时调用
     // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
     // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
     // 可以访问组件实例 `this`
   },
   beforeRouteLeave (to, from, next) {
     // 导航离开该组件的对应路由时调用
     // 可以访问组件实例 `this`
   }
 }
 beforeRouteEnter守卫不能访问this，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。
 可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
 beforeRouteEnter (to, from, next) {
 next(vm => {
   // 通过 `vm` 访问组件实例
 })
 }
 ```
注：对于beforeRouteUpdate和beforeRouteLeave路由组件已经创建了，即this可以使用。
  其中beforeRouteLeave通常用于用户还未保存就要离开时，next(false)取消跳转

#### 完整的导航解析流程

- 导航被触发。
- 在失活的组件里调用离开守卫。
- 调用全局的 beforeEach 守卫。
- 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
- 在路由配置里调用 beforeEnter。
- 解析异步路由组件。
- 在被激活的组件里调用 beforeRouteEnter。
- 调用全局的 beforeResolve 守卫 (2.5+)。
- 导航被确认。
- 调用全局的 afterEach 钩子。
- 触发 DOM 更新。
- 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。
