### 一.vue实例

new出来的即为实例

#### 1.vue实例的创建和作用

template会被编译为一个render function

el是挂载到哪个节点上

template就和引入组件差不多

**将template的内容挂载到div元素上，挂载过程是会把整个节点替换掉，因此在网页上看不到#root的元素**

```
import Vue from 'vue'
 
const div = document.createElement('div')
document.body.appendChild(div)
 
new Vue({
  el: div,
  template: '<div>this is content {{text}}</div>',
  data: {
    text: 'text'
  }
})
```

先定义document节点，再append到body中的方式，相对麻烦

可以注意到先加载了js，再加载真正的页面元素，比较奇怪

在真实的生产环境中，不建议这么操作，建议新建一个template.html文件

![](.\vue-img\vue基础\1.JPG)

 修改 webpack.config.practice.js 配置文件，生成HTML文件时会根据 template.html为模板生成 

![](.\vue-img\vue基础\2.JPG)

有了这个template模板文件之后， 

##### 方式一

```
import Vue from 'vue'
 
new Vue({
  el: '#root',
  template: '<div>this is content {{text}}</div>',
  data: {
    text: 'text'
  }
})
```

##### 方式二

```
import Vue from 'vue'
 
const app = new Vue({
  template: '<div>this is content {{text}}</div>',
  data: {
    text: 'text'
  }
})
app.$mount('#root')或者app.$mount('#app')
```

其实`render: h => h(App)`是

```
render: function (createElement) {
    return createElement(App);
}
```

#### 2.vue实例的属性

##### app.$data

就是vue中data里的数据

##### app.$props

父传子定义的props属性

##### app.$el 

挂载节点

##### app.$options 

传进入和默认的，可以获取在自定义的和本身的属性和方法

直接修改app.$options是没有作用的，app.$data可以
有一个方法 

```
app.$options.render = (h) => {
    return h('div', {}, 'new render function')
}
```

##### app.$root 

app.$root == app  true
赋值有用但是要等下一次重新赋值渲染的时候才有变化

##### app.$children

在某组件下的子节点

##### app.$slots

  插槽

##### app.$scopedSlots

作用域插槽 

#####  app.$refs 

引用dom对象

- ref 加在普通的元素上，用this.$refs.（ref值） 获取到的是dom元素
- ref 加在子组件上，用this.$refs.（ref值） 获取到的是组件实例，可以使用组件的所有方法。在使用方法的时候直接this.$refs.（ref值） .方法（） 就可以使用了。
- ref 需要在dom渲染完成后才会有，在使用的时候确保dom已经渲染完成。比如在生命周期 mounted(){} 钩子中调用，或者在 this.$nextTick(()=>{}) 中调用。 
- 组件不是DOM元素，没有dom元素的基本属性和方法，比如clientWidth 是没有的，就需要通过$el来获取组件中的DOM元素 ，this.$refs.（ref值）.$el.clientWidth 

#####  app.$isServer  

服务器端渲染去判断 

#### 3.vue实例的方法

##### app.$watch

app.$watch 和vue项目组件中watch的作用一样的

##### unWatch()    

注销watch   unWatch()         在vue项目option中的watch会自动注销

##### app.emit()

监听事件（触发test去监听这个事件）

app.$on('test', () => {

  console.log('test once')

})

触发事件 app.$emit('test')   on和emit只能同时作用一个vue对象

```
app.$on('test', () => {
  console.log('test once')
})
触发事件
app.$emit('test')   
on和emit只能同时作用一个vue对象

也可以监听的同时传参数
app.$on('test', (a,b) => {
  console.log(`test emited $(1) $(2)`)
})
触发事件
app.$emit('test'，1，2)   
```

##### app.$once()

// 监听 只监听一次

```
app.$once('test', () => {
  console.log('test once')
})
```

注意vue是响应式的 ，data里的对象属性只定义（为空）去调用不存在的一个值 ，值在变化，但不会重新渲染

##### app.$forceUpdate()

// 不建议用，强制组件渲染，不建议使用，若没控制好频度，会一直强制渲染，影响性能

#####  app.$set()

可以这样子改app.$set(app.obj，‘a’，i)

**//语法糖[vm.$set( target, key, value )]** 

##### app.$delete()

app.$delete(app.obj,'a')

##### app.$nextTick ()

如果你连续很多次去修改一个值，它不是每一次都去更新，而是等一个异步队列全部更新完

如果中间有某次更新后渲染完成需要去操作dom节点就可以用app.$nextTick ()

- [x] $nextTick要高于获取后端数据之类的异步api，因此需要在后端异步api的then方法中，执行$nextTick，then(...)方法之外会导致数据未更新。 

**理以上实例在vue组件中是一样的道理，只不过是直接用this引用**

### 二.vue生命周期

也就是vue对象生命周期

#### 1.有哪些生命周期方法

```
创建期间的生命周期函数：
  	+ beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
  	+ created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
  	+ beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
  	+ mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示
运行期间的生命周期函数：
 	+ beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
 	+ updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！
keep-alive特有的生命周期函数：
	+ activated
	+ deactivated
销毁期间的生命周期函数：
 	+ beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
 	+ destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
```

#### 2.生命周期方法的调用顺序

创建前/后, 载入前/后,更新前/后,销毁前/销毁后

#### 3.生命周期VUE实例有哪些区别

在new一个vue方法中，beforeCreate、created、挂载后（指定el）执行beforeMount、mounted

app.$mount也一样，mounted

如果在beforeCreate、created去打印this.$el为undefined 

beforeMount拿到节点  mounted拿到data

**以上4个生命周期只会被调用一次，服务的渲染时调用的只有beforeCreate、created（因为服务端执行时根本没有dom环境）**

beforeUpdate、updated要有数据更新才会执行，第一次刷新页面的生命周期不执行。只有数据更新虚拟dom监听到才执行

补充组件销毁方法 app.$destroy()

beforeDestroy、destroyed

![](.\vue-img\vue基础\3.JPG)

![](.\vue-img\vue基础\4.JPG)

打印的内容在beforeMount、mounted之间出现

如图，有没有template属性这一步，如果有的话，就去执行render function，在项目中写的vue文件都没有template，而是通过vue-loader直接变成了render function，beforeMount即在有了render function去执行

renderError方法

![](.\vue-img\vue基础\5.JPG)

keep-alive多出来的两个active及其销毁的生命周期

思考：注意比较导卫首航和create的先后顺序

### 三.vue的数据绑定

在data里的可以访问 全局的不能访问

v-on/@ 绑定事件

：class ：style动态绑定 写对象或者数组

vue绑定style 属性会自动加补贴浏览器的前缀

![](.\vue-img\vue基础\6.JPG)

数组和object合并

![](.\vue-img\vue基础\7.JPG)

可以用方法去取arr并用模板渲染出来

![](.\vue-img\vue基础\8.JPG)

### 四.computed和watch使用场景

#### 1.computed

computed会把数据缓存起来，提高性能，它所依赖的值有变化时才会去更新

computed不推荐去设值，因为它是一个多重组合起来的值，去设置需要把此值拆开来设置，因此不推荐用computed的set方法

get方法：![](.\vue-img\vue基础\9.JPG)

#### 2.watch

![](.\vue-img\vue基础\10.JPG)

注意computed是一个值，watch更像是一个方法，当immadiate为false的时候，第一次不会去执行它，当数据更新时才会执行，当immadiate为true时，会初始化执行一次

watch不适合去单纯显示某个数据，此时用computed代码更少，性能也会更好

watch更适合你监听到了某个数据的变化，你要取执行某个操作

watch里还有个属性deep，默认为false

![](.\vue-img\vue基础\12.JPG)

![](.\vue-img\vue基础\11.JPG)

- [x] v-model绑定obj.a并去改变 并没有触发handler，原因是它是一个对象，只有给对象赋值的时候，才监听得到

![](.\vue-img\vue基础\13.JPG)

此时加上deep:true，即是深入观察，相当于一层层便利了一遍，性能开销很大

或者：![](.\vue-img\vue基础\14.JPG)

特别注意！！！！！！！无论是在computed还是在watch都不要去修改你依赖的值，特别是computed，你只能尽量做到去生成新的值，不能修改原来的值，否则会无限循环

比如：![](.\vue-img\vue基础\15.JPG)

### 五.vue原生指令

![](.\vue-img\vue基础\16.JPG)

#### v-text

不支持在标签里写其他的东西，除非拼接

#### v-html 

会把标签显示出来

#### v-show

相当于在节点加display：none

#### v-if

相当于把整个节点move掉

so，如果你只是想展示隐藏显示，推荐v-show，因为v-if相当于动态增删，性能更差，耗时

#### v-for

遍历 注意要加key值

原因：希望是唯一的  一般像数据就有唯一的id，去绑定key，这个dom节点就复用了，不会重新渲染，节省开销

key是动态的，一般不推荐index为key，可以用item，有可能会导致错误的缓存（顺序）

 “(xx ,index)in arr”

 “(val,key,index)in obj”

如果我们不想多创节点，又想实现for循环，我们就可以用template来做渲染，在chorme开发者工具上，也能证实，最外层的的template会自动消失，不会创造出多余的节点 

```
<template>
     <div>
         <template v-for="item in list">
              <p>{{item.content}}</p>
              <img :src="item.img" alt="">
              <p class="divider"></p>
         </template>
     </div>
</template>

作者：混元霹雳手
链接：https://juejin.im/post/58d5dc9b44d90400686aacb4
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### v-on

@ 事件绑定

#### v-bind

：动态

#### v-model

数据绑定

![](.\vue-img\vue基础\17.JPG)

#### v-cloak 

v-cloak在直接引用vue.js才用得到，在vue代码还未加载完成之前，隐藏掉

#### v-once

v-once 数据绑定的内容只执行一次，用于静态部分，不需要去对比虚拟dom，节省性能

只进行第一次的数据渲染，如果再次改变值，文本值也不会改变 

应用场景 ： 一般是用在组件树中传递时，导致组件数据一层一层传递时，变改了不需要改变的场景，用v-once可以避免在组件数中只需用一次性赋值操作 

#### 修饰符

sync<https://www.jianshu.com/p/d42c508ea9de> 

<https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6> 

v-model.trim去掉数据首位空格

v-model.lazy 转变为在“change”时而非“input”时更新 

##### .native将原生事件绑定到组件

<https://cn.vuejs.org/v2/guide/components-custom-events.html#%E5%B0%86%E5%8E%9F%E7%94%9F%E4%BA%8B%E4%BB%B6%E7%BB%91%E5%AE%9A%E5%88%B0%E7%BB%84%E4%BB%B6> 

在父组件去监听子组件的事件，或者去直接绑定@相应事件

### 六.vue组件的定义

1.vue.js中全局定义组件

![](.\vue-img\vue基础\18.JPG)

推荐命名：组件命名类似类，大写开头，使用的时候用连接符

2.局部注册

![](.\vue-img\vue基础\19.JPG)

注意是：定义组件时data必须是函数

props：用于定义组件在外部使用时可变（配置）的一些行为！！！

在传布尔值的时候要加：动态，不然传入的就是字符串

props命名规则同组件

props不能主动去修改它，它是父节点对子节点的定义，推荐单向数据流

props数据验证

一般有了default就不需要required

![](.\vue-img\vue基础\120JPG.JPG)

### 七.vue组件的继承

相当于组件注册

![](.\vue-img\vue基础\22.JPG)

![](.\vue-img\vue基础\23.JPG)

继承这种方式传参：propsData

![](.\vue-img\vue基础\24.JPG)

继承这种方式，去定义data会覆盖掉component里的data，而mounted并不会覆盖，会先打印component里的mounted再去打印继承里new的mounted

![](.\vue-img\vue基础\25.JPG)

可继承、可扩展自定义组件

$.parent 调用父组件  可修改父组件的，但不建议

$.parent 去引用到调用子组件的组件（定义name）

调用name $parent.options.name

![1566493710827](C:\Users\ADMINI~1\AppData\Local\Temp\1566493710827.png)

第一个打印的是new里面的，指定parent名

声明一个组件：

![1566493782439](C:\Users\ADMINI~1\AppData\Local\Temp\1566493782439.png)

![1566493839396](C:\Users\ADMINI~1\AppData\Local\Temp\1566493839396.png)

### 八.vue组件双向绑定

v-model功能

![](.\vue-img\vue基础\26.JPG)

![](.\vue-img\vue基础\27.JPG)

![1566494936193](C:\Users\ADMINI~1\AppData\Local\Temp\1566494936193.png)

![](.\vue-img\vue基础\28.JPG)



既要双向绑定 又要加value去实现其他功能（定义变量名称）

![](.\vue-img\vue基础\29.JPG)

### 九.高级属性

#### 1.具名插槽

![](.\vue-img\vue基础\30JPG.JPG)

![](.\vue-img\vue基础\31.JPG)

#### 2.作用域插槽slot-scope

![](.\vue-img\vue基础\32.JPG)

这里的value一般只能是当前data里的value，但是当希望使用其他组件里的属性的时候

![](.\vue-img\vue基础\33.JPG)

![](.\vue-img\vue基础\34.JPG)

如果打印refs

![](.\vue-img\vue基础\35.JPG)

![](C:\Users\Administrator\Pictures\vue\vue基础\36.JPG)

第一个refs是一个实例，第二个是一个节点

也可以直接在ref上打印变量或方法，但不推荐直接以ref的改变实例的方式去使用变量或方法，因为你可以用props去操控组件

![](C:\Users\Administrator\Pictures\vue\vue基础\37.JPG)

实际开发中，作用域插槽一般应用于某些ui框架组件的自定义部分

通过template和scope-slote在原ui框架组件内部通过slot name 或者v-slot去占位

#### 3.provide、inject

爷爷辈（越级、跨层级）

父组件provide的时候，this还没有初始化成功，要用方法

![](.\vue-img\vue基础\39.JPG)

![](.\vue-img\vue基础\38.JPG)

在“孙子”组件中，this.parent.$option只能调用上级，也就是父级的属性，不能调用更上层级，此时要用注入inject

![](.\vue-img\vue基础\40.JPG)

子（孙）组件中引用的provide中的属性，不提供reactive属性，所以如果根组件改变不会重新渲染跟着改变

![](.\vue-img\vue基础\42.JPG)

![](.\vue-img\vue基础\41.JPG)

要这样改写去实现reactive

![](.\vue-img\vue基础\43.JPG)

defineProperty给data指定value，相当于子（孙）组件每次调用value值就调用get方法，获取最新的value值

![](.\vue-img\vue基础\44.JPG)

以上方法并不是很推荐，因为以后的vue版本很有可能被改写

### 十.render function

#### 1.渲染过程

把template的字符串通过render进行编译

$.createElement()是vue的一个创建节点的方法

它创建出来的并不是真正的节点，而是虚拟dom，*VNode*  

![](.\vue-img\vue基础\45.JPG)

1和2一样的效果

如果template里有子节点，那么render第三部分必须使用数组才会渲染出来

render slot

![](.\vue-img\vue基础\46.JPG)

slot没指定名字时就用$slot.default

#### 2.nativeOn和on区别

![](.\vue-img\vue基础\48.JPG)

![](.\vue-img\vue基础\47.JPG)

nativeOn如果creaElement是一个原生节点就直接绑定到节点上，如果是一个组件，就绑定到组件的根节点上，就不用在子组件emit去触发

#### 3.render中的slot

![](.\vue-img\vue基础\49.JPG)

![](.\vue-img\vue基础\50.JPG)

4.render中其他属性

加attr

![](.\vue-img\vue基础\51.JPG)

domProps

![](.\vue-img\vue基础\52.JPG)

### 十一.vue-router

vue-router自动在url后加#，默认hash模式

#### 1.默认路由跳转redirect

![](.\vue-img\vue基础\53.JPG)

#### 2.history

加了这个后 路径不会带#，这个模式需要后端配合匹配所有的路由 

![](.\vue-img\vue基础\54.JPG)

#### 3.base

基本路径/base/，不是强制性，加上后跳转的时候所有url都会自动加上这个

但是去掉这个，路径仍能跳转

![](.\vue-img\vue基础\55.JPG)

4.linkActiveClass和linkExactActiveClass

控制跳转的a标签的样式

![](.\vue-img\vue基础\56.JPG)

![](.\vue-img\vue基础\58.JPG)

![57](.\vue-img\vue基础\57.JPG)

#### 4.historyApiFallback（webpack）

当用户单击刷新按钮或直接通过输入地址的方式访问页面时，会出现找不到页面的问题，因为这两种方式都绕开了History API ，而我们的请求又找不到后端对应的路由，页面返回404错误。 connect-history-api-fallback中间件很好的解决了这个问题。具体来说，每当出现符合以下条件的请求时，它将把请求定位到你指定的索引文件(默认为/index.html)。 

和PublicPath 有关

![](.\vue-img\vue基础\59.JPG)

#### 5.scrollBehavior 

用于跳转的时候，页面要不要滚动

scrollBehavior (to, from, savedPosition) {

​    return { x: 0, y: 0 }

  }

to：要跳转的路由

from：从哪个路由

savedPosition：记录滚动条滚动的位置

一般这样使用：

![](.\vue-img\vue基础\60.JPG)

去哪儿网的案例是始终滚动为0

#### 6.parseQuery、stringifyQuery

用于解析url查询参数

提供自定义查询字符串的解析/反解析函数 

![](.\vue-img\vue基础\61.JPG)

#### 7.fallback

一般设置fallback:true 

用于history模式，有些浏览器不支持情况下，vue 会自动 fallback 到 hash 模式 

#### 8.meta

和 HTML 中 header 部分的 meta 页面原信息类似，例如 description 有助于 seo 等，一般和路由没什么关系的配置可以写在这

meta: { title: 'this is app', description: 'xxx' } 

#### 9.子路由 children

 子路由使用，需要再上一级路由页面加上 router-view 显示，也就是嵌套路由的概念

#### 10.transition

过渡效果

```
<template>
  <div id="app">
    <transition name="fade"> // 使用 name  包裹整个路由，也可以到具体的组件去包裹
      <router-view />
    </transition>
  </div>
</template>
css部分：
.fade-enter-active, .fade-leave-acitve /* fade- */
  transition: opacity  0.3s
.fade-enter, .fade-leave-to
  opacity : 0
```

#### 11.传递参数（编程时导航）

path不能和param一起，否则param将不生效

```
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

//也可以拼接path的方式去实现param
router.push({ path: `/user/${userId}` }) // -> /user/123
```

#### 12.接收参数

$this.route  

$this.route .param

一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。 

如果说路由index.js中没有加:id这种形式,刷新后参数会消失，$this.route .param.id为undefined

$this.route .query

一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象 

#### 13.props

![](.\vue-img\vue基础\62.JPG)

props为true时，把id作为props传入todo组件，更解耦并且更灵活

![](.\vue-img\vue基础\63.JPG)

#### 14.vouter-view

当有多个router-view时，用components，可以适用于上中下，左右布局

![](.\vue-img\vue基础\64.JPG)

#### 15.导航守卫

1.在router的index.js中 全局路由有以下几个导航钩子

router.beforeEach((to,from,next))=>{})中执行next()，路由才会跳转

before可以做校验，可以用于用户登录验证

![](.\vue-img\vue基础\65.JPG)

router.beforeResolve(to,from,next))=>{})

跳转完成后的执行函数，没有next了

router.afterEach(to,from))=>{})

2.在路由配置中，路由钩子beforeEnter 进入某个路由之前才会被调用，它的执行顺序在beforeEach和beforeResolve之间

![](.\vue-img\vue基础\66.JPG)

3.组件路由中

只有beforeRouteEnter钩子没有this

```
router.beforeRouteEnter((to,from,next))=>{

next()

})

router.beforeRouteUpdate((to,from,next))=>{

next()

})

router.beforeRouteLeave((to,from,next))=>{

next()

})

```

![](.\vue-img\vue基础\68.JPG)

执行顺序

跳到默认页

beforeRouteLeave=》beforeEach=》beforeResolve=》afterEach

跳到另一个页面

beforeEach=》beforeEnter=》beforeRouteEnter=》beforeResolve=》afterEach

![](.\vue-img\vue基础\67.JPG)

全局》路由配置》组件

router.beforeRouteUpdate：同样的组件在不同的复用下（同样的路由形式切换），比如每次id有变化时去进行数据更新，不用watch，减小开销或者获取出错撤销

next操作

![](.\vue-img\vue基础\69.JPG)

用beforeRouteLeave去控制离开

![](.\vue-img\vue基础\70.JPG)

#### 16.异步组件

首屏加载的时候，速度更快，component接收的是一个函数

![](.\vue-img\vue基础\72.JPG)

插件babel-plugin-syntax-dyntax-dynamic-import去识别这种引用语法

![](.\vue-img\vue基础\73.JPG)



### 十二.vuex

#### 1.基础结构

单向数据流

首先在main.js引入store

![](.\vue-img\vue基础\74.JPG)

commit去改变

getters相当于computed

引入各个流程的文件后直接缩写声明

![](.\vue-img\vue基础\75.JPG)

#### 2.state和getters

两个帮助方法mapState，mapGetters辅助函数，都写在computed里

...这种结构扩展运算符（使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 `computed` 属性 

） 本身vue不支持 ，框架里.babelrc引入了babel-preset-stage-1

使用mapState后如果同名 使用数组

![](.\vue-img\vue基础\77.JPG)

如果不同名，使用字符串形式

![](.\vue-img\vue基础\78.JPG)

**更好的方式，使用方法**

![](.\vue-img\vue基础\79.JPG)

mapGetters1和2方式一样

![](.\vue-img\vue基础\80.JPG)

#### 3.mutation和action

commit触发mutation

commit只能传2个参数（payload）

如果是多个参数，可以弄成一个对象

![](.\vue-img\vue基础\84.JPG)

![](.\vue-img\vue基础\82.JPG)

不推荐直接修改，要用mutation进行修改

![](.\vue-img\vue基础\83.JPG)

严格模式只能在开发环境加这个，加了后就不能直接改

mutation里只能放同步更新的代码，如果有异步的代码（比如数据请求）要去action里，用dispatch触发action

![](.\vue-img\vue基础\86.JPG)

![](C:\Users\Administrator\Pictures\vue\vue基础\87.JPG)

mapMutations和mapActions写在methods里

![](.\vue-img\vue基础\90.JPG)

![](C:\Users\Administrator\Pictures\vue\vue基础\89.JPG)

#### 4.vuex模块

namespaced: true, 

加了后 每一个模块都可以写相同名字的mutations和actions

![](.\vue-img\vue基础\91.JPG)

![](.\vue-img\vue基础\92.JPG)

getters：

![](.\vue-img\vue基础\94.JPG)

![](.\vue-img\vue基础\93.JPG)

用以上数组的方式不好用模板语法，因此更改为对象形式

![](.\vue-img\vue基础\95.JPG)

拿到全局模式下的state或其他模块的state

getters方法内第二个参数是所有getter方法，第三个参数rootState是全局的state方法

actions里方法第一个参数是ctx，这是个store对象，包含state，commit，rootState

![](.\vue-img\vue基础\100.JPG)

![](.\vue-img\vue基础\96.JPG)

要注意此时commit的updateText是默认action这个所在的命名空间里的updateText

如果你要调用其他模块或者全局里的mutations，需要加root:true，不然会报错（前提是声明了namespaced: true）

![](.\vue-img\vue基础\97.JPG)

![](.\vue-img\vue基础\98.JPG)

![](.\vue-img\vue基础\101.JPG)

![](.\vue-img\vue基础\99.JPG)

还可以模块下增加模块

##### 动态注册模块

在index.js中全局路由去添加

![](.\vue-img\vue基础\103.JPG)

#### 5.热更替

没有加热更替修改mutation 都会刷新下页面

官方文档：

使用require引入 因为import只能在外层引入，不能在代码内部引入，并且必须加上.default

注意store即是前面创建的store

```
const store = new Vuex.Store({
  state,
  mutations,
  modules: {
    a: moduleA
  }
})
if (module.hot) {
  // 使 action 和 mutation 成为可热重载模块
  module.hot.accept(['./mutations', './modules/a'], () => {
    // 获取更新后的模块
    // 因为 babel 6 的模块编译格式问题，这里需要加上 `.default`
    const newMutations = require('./mutations').default
    const newModuleA = require('./modules/a').default
    // 加载新模块 
    store.hotUpdate({
      mutations: newMutations,
      modules: {
        a: newModuleA
      }
    })
  })
}
```

实例

![](.\vue-img\vue基础\104.JPG)

![](.\vue-img\105.JPG)

#### 6.store.watch

第一个参数相当于getter

![](.\vue-img\vue基础\106.JPG)

当state变化时，会自动调用store.watch方法

#### 7.store.subscribe订阅

制作一些插件时

![](.\vue-img\vue基础\107.JPG)

拿到回调函数，type打印调用了哪个mutation

payload打印接收（传入）的参数

![](.\vue-img\vue基础\82.JPG)

![](.\vue-img\vue基础\108.JPG)

store.subscribeAction

![](.\vue-img\vue基础\109.JPG)

#### 8.plugin

![](.\vue-img\vue基础\110.JPG)

### 十三.混入mixin

#### Mixins

可以定义共用的变量，在每个组件中使用，引入组件中之后，各个变量是相互独立的，值的修改在组件中**不会相互影响** 

mixins执行的顺序为mixins>mixinTwo>created(vue实例的生命周期钩子); 

当混合里面包含异步请求函数，而我们又需要在组件中使用异步请求函数的返回值时，我们会取不到此返回值 

解决方案：不要返回结果而是直接返回异步函数 

<https://www.cnblogs.com/Ivy-s/p/10022636.html> 

#### extends

<https://segmentfault.com/a/1190000015608340> 

extends执行顺序为:extends>mixins>mixinTwo>created 2.定义的属性覆盖规则和mixins一致 

### 十四.MVVM模式解析

#### 1.vue和jquery区别

- 数据和视图的分离，解耦（开放封闭原则）
- 以数据驱动视图，只关心数据变化，DOM操作被封装

#### 2.对比MVC和MVVM

mvc：controller--》model--》view

mvvm：model（数据模型data）=》viewModel=》view

viewModel双向数据绑定

#### 3.vue三要素

**响应式**

vue如何监听到data的变化

**模板引擎**

vue模板如何被解析，指令如何处理

**渲染**

vue的模板如何被渲染成html

#### 4.vue如何实现响应式

修改data属性之后，vue立刻被监听到，利用Object.defineProperty代理到vm对象上

```
let vm = {}；//实例
const data = {
    price: 100,
    name: 'zs'
};
let key,value;//缓存dom查询
for(key in data){
	//闭包，新建一个函数，保证key的独立的作用域 每次都有新的值
    (function(key){
    	//通过defineProperty代理到vm对象上
        Object.defineProperty(vm, key, {
            get(){
                return data[key];
            }
            ser(newVal){
                data[key] = newVal;
            }
        })
    })(key)
}
console.log(vm.name = xx)//监听到vm对象上name值的改变
```

#### 5.vue如何解析模板

##### 模板

本质：字符串

有逻辑：如v-if、v-for等

嵌入js变量：模板最终要转换成html来显示，必须用js才能实现（render函数）

##### render函数

**with的使用**

自己开发的代码中with尽量不要用

```
function fn(){
    with(obj){
        alert(age)
        alert(name)
        gatNum()
    }
}
fn();
```

![](.\img\10.JPG)

this即vm

price即this.price即vm.price

_c即this. _c即  vm. _c

_c即是createElement

_v即是createTextNode文本节点

_s即是toString

![](.\img\11.JPG)

**render函数如何处理模板的**

在vue2.6版本源码中找到

var code = generate(ast, options);

console.log(code.render)就可以看到模板渲染成render的形式

![](.\img\12.JPG)

![](.\img\15.JPG)

directive即指令

“  ”表示换行

list是个数组进行遍历

**render函数返回了什么**

利用虚拟vdom，render返回的是vnode

对比新旧vnode，第一次没有对比的时候把旧cnode全部渲染在vm.$el

![1574971705738](C:\Users\ADMINI~1\AppData\Local\Temp\1574971705738.png)

**流程**

- updateComponent中实现了vdom的patch
- 页面首次渲染执行updateComponent
- data每次修改属性，set中执行updateComponent

#### 3.整体流程

![](.\img\14.JPG)

![](.\img\16.JPG)

![](.\img\17.JPG)

![](.\img\18.JPG)

### 补充：Webpack4搭建项目

<https://segmentfault.com/a/1190000006178770> 

4.package.json

```
{
  "name": "webpack4",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:client": "cross-env NODE_ENV=production webpack --config webpack.config.client.js",
    "build": "npm run clean && npm run build:client",
    "clean": "rimraf dist",
    "lint": "eslint --ext .js --ext .jsx --ext.vue",
    "lint-fix": "eslint --fix --ext .js --ext .jsx --ext.vue",
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.client.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "vue": "^2.6.10"
  },
  "devDependencies": {
    "@babel/core": "^7.6.4",
    "@babel/preset-env": "^7.6.3",
    "autoprefixer": "^9.6.4",
    "babel-eslint": "^10.0.3",
    "babel-helper-vue-jsx-merge-props": "^2.0.0",
    "babel-loader": "^8.0.6",
    "babel-plugin-transform-vue-jsx": "^3.7.0",
    "cross-env": "^6.0.3",
    "css-loader": "^3.2.0",
    "eslint": "^6.5.1",
    "eslint-config-standard": "^14.1.0",
    "eslint-loader": "^3.0.2",
    "eslint-plugin-html": "^6.0.0",
    "eslint-plugin-import": "^2.18.2",
    "eslint-plugin-node": "^10.0.0",
    "eslint-plugin-promise": "^4.2.1",
    "eslint-plugin-standard": "^4.0.1",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "file-loader": "^4.2.0",
    "html-webpack-plugin": "^3.2.0",
    "husky": "^3.0.9",
    "i": "^0.3.6",
    "npm": "^6.12.0",
    "postcss-loader": "^3.0.0",
    "rimraf": "^3.0.0",
    "style-loader": "^1.0.0",
    "stylus": "^0.54.7",
    "stylus-loader": "^3.0.2",
    "url-loader": "^2.2.0",
    "vue-loader": "^15.7.1",
    "vue-style-loader": "^4.1.2",
    "vue-template-compiler": "^2.6.10",
    "webpack": "^4.41.0",
    "webpack-cli": "^3.3.9",
    "webpack-dev-server": "^3.8.2",
    "webpack-merge": "^4.2.2"
  }
}
```

//热重载.vue文件要使用vue-style-loader

#### eslint配置

标准规则

npm i eslint eslint-config-standard eslint-plugin-standard eslint-plugin-promise eslint-plugin-import eslint-plugin-node -D

extract-text-webpack-plugin

解析.vue文件

npm i eslint-plugin-html

npm i eslint-loader babel-eslint -D

新建.eslintrc文件

```
{
	"extends": "standard",
	"plugins": [
		"html"
	],
	"parser": "babel-eslint"
}
```

package.json中

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:client": "cross-env NODE_ENV=production webpack --config webpack.config.client.js",
    "build": "npm run clean && npm run build:client",
    "clean": "rimraf dist",
    "lint": "eslint --ext .js --ext .jsx --ext.vue",
    "lint-fix": "eslint --fix --ext .js --ext .jsx --ext.vue",
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.client.js"
  }
```

新建.editorconfig文件， 用来规范编辑器规范

```
root = true

[*]
charset = utf-8
# 行尾符
end_of_line = 1f
# tab键两个字符
indent_size = 2
# 字表符
indent_style = space
# 自动加上空行
insert_final_newline = true
# 消除空格
trim_tralling_whitespace = true
```

提交git前进行检查

初始化git

npm i husky -D

"precommit": "npm run lint-fix"

#### webpack4升级

配置主要有以下几个地方

1.config 里mode模块

2.CommonsChunkPlugin删除之后，改成使用`optimization.splitChunks`进行模块划分

对entry进行拆分，需要设置`optimization.splitChunks.chunks = 'all'`，并且不再需要设置vendor

`optimization.runtimeChunk`：true就会自动拆分runtime文件

3.webpack4会自动加devtool 

4.webpack.NoEmitOnErrorsPlugin 取消了

webpack.config.client.js学习

```
const path = require('path')
const HTMLPlugin = require('html-webpack-plugin')
const webpack = require('webpack')
const merge = require('webpack-merge')
const ExtractPlugin = require('extract-text-webpack-plugin')
const baseConfig = require('./webpack.config.base')

const isDev = process.env.NODE_ENV === 'development'

const defaultPluins = [
  new webpack.DefinePlugin({
    'process.env': {
      NODE_ENV: isDev ? '"development"' : '"production"'
    }
  }),
  new HTMLPlugin()
]

const devServer = {
  port: 8000,
  host: '0.0.0.0',
  overlay: {
    errors: true,
  },
  hot: true
}

let config

if (isDev) {
  config = merge(baseConfig, {
    devtool: '#cheap-module-eval-source-map',
    module: {
      rules: [
        {
          test: /\.styl(us)?$/,
          use: [
            'vue-style-loader',
            'css-loader',
            {
              loader: 'postcss-loader',
              options: {
                sourceMap: true,
              }
            },
            'stylus-loader'
          ]
        }
      ]
    },
    devServer,
    plugins: defaultPluins.concat([
      new webpack.HotModuleReplacementPlugin()
      // new webpack.NoEmitOnErrorsPlugin()
    ])
  })
} else {
  config = merge(baseConfig, {
    entry: {
      app: path.join(__dirname, '../client/index.js')
      // vendor: ['vue']
    },
    output: {
      filename: '[name].[chunkhash:8].js'
    },
    module: {
      rules: [
        {
          test: /\.styl(us)?$/,
          use: ExtractPlugin.extract({
            fallback: 'vue-style-loader',
            use: [
              'css-loader',
              {
                loader: 'postcss-loader',
                options: {
                  sourceMap: true,
                }
              },
              'stylus-loader'
            ]
          })
        }
      ]
    },
    optimization: {
      splitChunks: {
        chunks: 'all'
      },
      runtimeChunk: true
    },
    plugins: defaultPluins.concat([
      new ExtractPlugin('styles.[hash:8].css'),
      // new webpack.optimize.CommonsChunkPlugin({
      //   name: 'vendor'
      // }),
      // new webpack.optimize.CommonsChunkPlugin({
      //   name: 'runtime'
      // })
    ])
  })
}

module.exports = config
```

webpack.config.base.js

```
const path = require('path')
const createVueLoaderOptions = require('./vue-loader.config')

const isDev = process.env.NODE_ENV === 'development'

const config = {
  mode: process.env.NODE_ENV || 'production', // development || production
  target: 'web',
  entry: path.join(__dirname, '../client/index.js'),
  output: {
    filename: 'bundle.[hash:8].js',
    path: path.join(__dirname, '../dist')
  },
  module: {
    rules: [
      {
        test: /\.(vue|js|jsx)$/,
        loader: 'eslint-loader',
        exclude: /node_modules/,
        enforce: 'pre'
      },
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: createVueLoaderOptions(isDev)
      },
      {
        test: /\.jsx$/,
        loader: 'babel-loader'
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(gif|jpg|jpeg|png|svg)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 1024,
              name: 'resources/[path][name].[hash:8].[ext]'
            }
          }
        ]
      }
    ]
  }
}

module.exports = config
```

postcss.config.js

```
const autoprefixer = require('autoprefixer')
//浏览器需要加前缀才能识别的css属性 autoprefixer自动处理
module.exports = {
  plugins: [
    autoprefixer()
  ]
}
```

### git配置

<https://blog.csdn.net/qq_41115965/article/details/81534512> 

<https://blog.csdn.net/zamamiro/article/details/70172900> 

### rollup打包

功能单一但极致，可集成可拓展

wangEditor集成用的rollup+gulp