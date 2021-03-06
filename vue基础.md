一.vue实例

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

##### app.$attrs

*包含了父作用域中不被props接收拿到的 (class 和 style 除外)。当一个组件没有声明任何 props 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind=”$attrs” 传入内部组件——在创建更高层次的组件时非常有用。*

##### app.$listeners

包含了父作用域中的 (不含 .native 修饰器的) 所有v-on 事件。它可以通过 v-on=”$listeners” 传入内部组件——在创建更高层次的组件时非常有用*

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

sync vue2.3+
子组件去改变props的值并且触发 不需要父组件去监听了
<https://www.jianshu.com/p/d42c508ea9de> 

<https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6> 
```
<student-list-info
  style="float: left"
  @confirm="onConfirm"
  :student="this.student"
  :visible.sync="display"
>
</student-list-info>

changeDisplay (value) {
  // this.visible = value
  // this.$emit('switch', value)
  this.$emit('update:visible', value)
}
```
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

![](.\vue-img\vue基础\28.JPG)



既要双向绑定 又要加value去实现其他功能（定义变量名称）

![](.\vue-img\vue基础\29.JPG)

### 九.状态管理bus的使用

#### 单向数据流

```
<template>
  <input @input="handleInput" :value="value"/>
</template>
<script>
export default {
  name: 'AInput',
  props: {
    value: {
      type: [String, Number],
      default: ''
    }
  },
  methods: {
    handleInput (event) {
      const value = event.target.value
      this.$emit('input', value)
    }
  }
}
</script>

父组件：
<a-input v-model="stateValue"/>
v-model实际上相当于t<a-input @input="handleInput" :value="inputValue"/>
data () {
    return {
      inputValue: ''
    }
},
methods: {
    handleInput (val) {
      this.inputValue = val
    }
}
```

#### 状态管理bus

```
main.js
import Bus from './lib/bus'
Vue.prototype.$bus = Bus

bus.js
import Vue from 'vue'
const Bus = new Vue()
export default Bus

组件
<button @click="handleClick">按我</button>
methods: {
    handleClick () {
    //兄弟组件传值
      this.$bus.$emit('on-click', 'hello')
    }
}

另一兄弟组件中：监听
this.$bus.$on('on-click', mes => {
      this.message = mes
})
```

 // "lint": "vue-cli-service lint",

### 十.高级属性

#### 1.具名插槽

封装的组件中： <slot name="title"></slot>

则引用组件的文件中可以写 <template slot="content">  xxx.... </template>

#### 2.作用域插槽slot-scope

![](.\vue-img\vue基础\32.JPG)

这里的value一般只能是当前data里的value，但是当希望使用其他组件里的属性的时候

![](.\vue-img\vue基础\33.JPG)

![](.\vue-img\vue基础\34.JPG)

如果打印refs

![](.\vue-img\vue基础\35.JPG)

第一个refs是一个实例，第二个是一个节点

也可以直接在ref上打印变量或方法，但不推荐直接以ref的改变实例的方式去使用变量或方法，因为你可以用props去操控组件

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

### 十一.render function

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

### 十二.vue-router

vue-router自动在url后加#，默认hash模式

#### 1.重定向redirect

```
  {
    path: '/main',
    name: 'main',
    meta: {
      title: 'main'
    },
    redirect: to => '/'
    /*或者redirect: to => {
    	return ‘/’
    }*/
    //to是代表当前要访问的页面的路由对象
  },
```

#### 2.history

无刷新页面

加了这个后 路径不会带#，这个模式需要后端配合匹配所有的路由 

```
export default new Router({
  routes,
  mode: 'history'
})
```

匹配不到路由资源会有问题，所以应该匹配到404 ，一定要定义到最后

```
 {
    path: '*',
    component: () => import('@/views/error_404.vue')
 }
```

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

#### 8.meta路由元信息

和 HTML 中 header 部分的 meta 页面原信息类似，例如 description 有助于 seo 等，一般和路由没什么关系的配置可以写在这

to.meta && setTitle(to.meta.title)

#### 9.子路由 children/嵌套路由

 子路由使用，需要再上一级路由页面加上 router-view 显示，也就是嵌套路由的概念

children写path的时候不需要加/，vue路径会自动加上  完整的路径应该是父path+子path

```
 children: [
      {
        path: 'table',
        name: 'table_page',
        meta: {
          title: '表格'
        },
        component: () => import('@/views/table.vue')
      },
 ]
```

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
  
  
//过渡组 为某个页面设置特定的动效
<transition-group :name="routerTransition">
    <router-view key="default"/>
    <router-view key="email" name="email"/>
    <router-view key="tel" name="tel"/>
</transition-group>


data () {
    return {
    	routerTransition: ''
    }
},
watch: {
    '$route' (to) {
    	//url拼接是query参数后 有了transition的名字
		to.query && to.query.transitionName && (this.routerTransition =  to.query.transitionName)
    }
},  
.router-enter{
  opacity: 0;
}
.router-enter-active{
  transition: opacity 1s ease;
}
.router-enter-to{
  opacity: 1;
}
.router-leave{
  opacity: 1;
}
.router-leave-active{
  transition: opacity 1s ease;
}
.router-leave-to{
  opacity: 0;
}
```

#### 11.传递参数（编程式导航）

path不能和param一起，否则param将不生效

```
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

//也可以拼接path的方式去实现param
router.push({ path: `/user/${userId}` }) // -> /user/123
```

#### 12.动态路由匹配

```
{
    path: 'params/:id',
    name: 'params',
    meta: {
    	title: '参数'
    },
    component: () => import('@/views/argu.vue')
}
```

push和relpace区别

 router.push 会向 history 栈添加一个新的记录，当用户点击浏览器后退按钮时，则回到之前的 URL

 router.replace 导航后不会留下 history 记录。即使点击返回按钮也不会回到这个页面 

#### 13.接收参数

$this.route  代表路由对象

$this.route .param

一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。 

如果说路由index.js中没有加:id这种形式,刷新后参数会消失，$this.route .param.id为undefined

$this.route .query

一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象 

#### 14.路由组件传参---props解耦

- 布尔类型---动态路由匹配

  props为true时，把id作为props传入todo组件，更解耦并且更灵活

```
{
        path: 'params/:id',
        name: 'params',
        meta: {
          title: '参数'
        },
        component: () => import('@/views/argu.vue'),
        props: true
}
```

- 对象模式

```
{
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '@/views/About.vue'),
    props: {
      food: 'banana'
    }
 },
```

- 函数模式

```
{
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '@/views/About.vue'),
    props: route => {
    	food: route.query.food
    }
 },
 
 组件中
 props:{
 	food:{
 		type:String,
 		default:'apple'
 	}
 }
```

#### 15.router-view 命名视图

视图渲染组件

当有多个router-view时，用components，可以适用于上中下，左右布局

显示多个试图

```
{
    path: '/named_view',
    name: 'named_view',
    meta: {
      title: 'named_view'
    },
    components: {
      default: () => import('@/views/child.vue'),
      email: () => import('@/views/email.vue'),
      tel: () => import('@/views/tel.vue')
    }
  }
```

#### 16.导航守卫

##### 全局守卫

  在router的index.js中 全局路由有以下几个导航钩子

- router.beforeEach

  router.beforeEach((to,from,next))=>{})中执行next()，路由才会跳转

  to、from都是路由对象

  beforeEach可以做校验，可以用于用户登录验证

  ```
  const HAS_LOGINED = false
  
  router.beforeEach((to, from, next) => {
    to.meta && setTitle(to.meta.title)
       if (to.name !== 'login') {
         if (HAS_LOGINED) next() //如果已经登录 默认跳到登陆页
         else next({ name: 'login' })
       } else {
         if (HAS_LOGINED) next({ name: 'home' })
         else next()
      }
  }
  ```

- router.beforeResolve

  在导航被确认之前（所有钩子都结束）触发


​       router.beforeResolve(to,from,next))=>{})

- router.afterEach 后置钩子，不能对跳转页面进行操作

  跳转完成后的执行函数，没有next了：router.afterEach(to,from))=>{})
  
  ```
  一般可用于处理loading
  router.afterEach((to, from, next) => {
    loading = false
  }
  ```

##### 路由独享守卫

​	在路由配置中，路由钩子beforeEnter 进入某个路由之前才会被调用，它的执行顺序在beforeEach和	beforeResolve之间

```
{
    path: '/login',
    name: 'login',
    meta: {
      title: '登录'
    },
    component: () => import('@/views/login.vue'),
    beforeEnter: (to, from, next) => {
    	if (form.name === 'login') console.log('这不是登陆页来的')
    	next()
    }
}
```

##### 组件内的守卫

- 只有beforeRouteEnter钩子没有this，还没有渲染，可以用vm参数，vm为组件实例
- router.beforeRouteUpdate：同样的组件在不同的复用下（同样的路由形式切换），比如每次id有变化时去进行数据更新，不用watch，减小开销或者获取出错撤销

```
{
    //xx组件,data\props.....
    beforeRouteEnter(to, from, next) {
    	next(vm => {
    	
    	})
    },
    beforeRouteLeave (to, from, next) {
        // const leave = confirm('您确定要离开吗？')
        // if (leave) next()
        // else next(false)
        next()
    },
    //路由发生变化，组件被复用时调用（同一url传参改变时适用）
    beforeRouteUpdate (to, from, next) {
        console.log(to.name, from.name)
        next()
    }
}
```

##### 完整的导航解析流程

 * 1. 导航被触发
 * 2. 在失活的组件（即将离开的页面组件）里调用离开守卫 beforeRouteLeave
 * 3. 调用全局的前置守卫 beforeEach
 * 4. 在重用的组件里调用 beforeRouteUpdate
 * 5. 调用路由独享的守卫 beforeEnter
 * 6. 解析异步路由组件
 * 7. 在被激活的组件（即将进入的页面组件）里调用 beforeRouteEnter
 * 8. 调用全局的解析守卫 beforeResolve
 * 9. 导航被确认
 * 10. 调用全局的后置守卫 afterEach（所有导航钩子完成）
 * 11. 触发DOM更新
 * 12. 用创建好的实例调用beforeRouterEnter守卫里传给next的**回调函数**

#### 17.异步组件/路由懒加载

只有访问到这个页面时，才会加载到这个路由

首屏加载的时候，速度更快，component接收的是一个函数

```
/*代表注释*/

component: () => import(/* webpackChunkName: "about" */ '@/views/About.vue')
```

插件babel-plugin-syntax-dyntax-dynamic-import去识别这种引用语法

![](.\vue-img\vue基础\73.JPG)

#### 18.router-link

tag代表真实标签

```
<router-link tag="li" to="/about">
  <a>About</a>
</router-link>
```

#### 19.别名 *alias* 

```
{
  path: '/admin',
  component: AdminPanel,
  alias: '/manage'
}
```

### 十三.vuex

#### 1.单向流程

 dispatch触发actions   commit触发mutations，mutations去改变state的状态

在store文件下创建index.js  new Vuex.Store定义各个流程

```
在main.js文件中引入store

import store from './store'

new Vue中注册

new Vue({
  router,
  store,
  created () {
    bootstrap()
  },
  render: h => h(App)
}).$mount('#app')
```

#### 2.state和getters

两个帮助方法mapState，mapGetters辅助函数，都写在computed里

...这种结构扩展运算符（使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 `computed` 属性 ） 本身vue不支持 ，框架里.babelrc引入了babel-preset-stage-1

##### state

```
//state
//组件中
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
computed: {
	appName () {
	  //获取根组件的state
      return this.$store.state.appName
    },
    //获取模块中的state user模块
    userName () {
       return this.$store.state.user.userName
    },
    //解构赋值写法 展开操作符
    //数组写法 同名可以写出这种 即命名名字和state里的名字一致
    ...mapState([
      'userName'
      //如果不同名，字符串写法
      anotherName：'userName'
    ]),
    //对象写法
    ...mapState({
      appName: state => state.appName,
      //模块下写法
      userName: state => state.user.userName
    }),
}
//vuex如果在模块中使用了命名空间，此模块更加密闭，不受干扰
export default {
    namespaced: true,
    getters,
    state,
    mutations,
    actions
}

```

##### createNamespacedHelpers

```
//另一种模块下的state写法，
import {createNamespacedHelpers } from "vuex";
const {mapState,mapGetters} = createNamespacedHelpers("user");
//第一个参数是可选的,传入模块名
//在 user 中查找
...mapState({
    appName: state => state.appName
}),
```

##### getter

```

//getter vuex中的getter相当于组件里的计算属性
//在store里新建getters.js
const getters = {
  appNameWithVersion: (state) => {
    //这里appName依赖于state中的字段
    return `${state.appName}v2.0`
  }
}
export default getters

//在vuex的user模块下定义getter
const getters = {
  firstLetter: (state) => {
  	//获取userName的
    return state.userName.substr(0, 1)
  }
}

//组件中使用getter
appNameWithVersion () {
	//获取根组件的getters
   return this.$store.getters.appNameWithVersion
},
//mapGetters函数中获取根组件的getters
...mapGetters([
    'appNameWithVersion'
]),
//获取user模块中的getters  没有开启命名空间的前提下，也可以不用写模块名
...mapGetters([
    'firstLetter'
]),
//开启命名空间后，
...mapGetters('user',[
    'firstLetter'
]),
//或者
...mapGetters([
    'user/firstLetter'
]),

//createNamespacedHelpers里
import {createNamespacedHelpers } from "vuex";
const {mapState,mapGetters} = createNamespacedHelpers("user");
...mapGetters('user',[
    'firstLetter'
]),
```

#### 3.mutation和action

dispatch触发action ，多用于异步请求

commit触发mutation

getters、mutations、actions不需要写模块名，vuex自动注册为全局，除非你想给它注册单独的命名空间

##### mutation

```
//mutation
//store.js下新建mutation.js
import vue from 'vue'
const mutations = {
  SET_APP_NAME (state, params) {
    //如果用写法2、3 就要写params.appName，如果是写法1  则直接=params即可
    state.appName = params.appName
    //如果用对象的写法 ，那么这个params参数就是一个对象，第一个参数就是type
  },
  SET_APP_VERSION (state) {
  	//如果state部分没有定义appVersion，则必须使用vue.set来进行新的值的视图更新
    vue.set(state, 'appVersion', 'v2.0')
  },
  SET_STATE_VALUE (state, value) {
    state.stateValue = value
  }
}
export default mutations

//组件中
handleChangeAppName () {
	//写法1 无参数
	this.$store.commit('SET_APP_NAME'),
    //写法2
    //这种写法只有2个参数
	this.$store.commit('SET_APP_NAME',{
         appName: 'newAppName'
     })
     //写法3  对象的写法可以有多个参数
	 this.$store.commit({
         type: 'SET_APP_NAME',
         appName: 'newAppName',
         .....
     })
}

//mutation辅助函数
import { mapMutations, mapActions } from 'vuex'
 methods: {
     ...mapMutations([
          'SET_USER_NAME'
    ]),
    handleChangeAppName () {
        //可以直接引用mutation中的函数
        this.SET_USER_NAME('vue-cource')
        //或者
        this.SET_USER_NAME({
            appName: 'vue-cource'
        })
    },
}
//模块中的mutations  user模块中
const mutations = {
  SET_USER_NAME (state, params) {
    state.userName = params
  },
  SET_RULES (state, rules) {
    state.rules = rules
  }
}
//组件中使用  getters、mutations、actions不需要写模块名，vuex自动注册为全局，除非你想给它注册单独的命名空间
this.SET_USER_NAME({
   appName: 'vue-cource'
})

```

如果是多个参数，可以弄成一个对象

![](.\vue-img\vue基础\84.JPG)

![](.\vue-img\vue基础\82.JPG)

不推荐直接修改，要用mutation进行修改

![](.\vue-img\vue基础\83.JPG)

严格模式只能在开发环境加这个，加了后就不能直接改

##### action

当 mutation 触发的时候，回调函数还没有被调用 
mutation里只能放同步更新的代码，如果有异步的代码（比如数据请求）要去action里，用dispatch触发action

```
const actions = {

 updateAppName ({ commit }) {
    getAppName().then(res => {
    //res.info.appName 就是 appName
      const { info: { appName } } = res
      commit('SET_APP_NAME', appName)
    }).catch(err => {
      console.log(err)
    })
  }
  //await写法
  async updateAppName ({ commit }) {
    try {
      const { info: { appName } } = await getAppName()
      commit('SET_APP_NAME', appName)
    } catch (err) {
      console.log(err)
    }
  }
}

//api.js模拟接口请求
export const getAppName = () => {
  return new Promise((resolve, reject) => {
    const err = null
    setTimeout(() => {
      if (!err) resolve({ code: 200, info: { appName: 'newAppName' } })
      else reject(err)
    })
  })
}


//组件中
 methods: {
 	//方式二
    ...mapActions([
        'updateAppName'
    ]),
    changeName(){
    	//方式一 直接使用dispatch
    	this.$store.dispatch('updateAppName', '123')
    	//调用mapActions里的函数
    	this.updateAppName('123')
    }
}

//模块如果有namespaced  要加上模块名
...mapActions('user',[
    'updateAppName'
]),
//或者用createNamespacedHelpers
import {createNamespacedHelpers } from "vuex";
const { mapActions } = createNamespacedHelpers("user");
//第一个参数是可选的,传入模块名
...mapActions([
	'updateAppName'
]),
```

#### 4.vuex模块module

namespaced: true, 

加了后 每一个模块都可以写相同名字的mutations和actions

![](.\vue-img\vue基础\91.JPG)


用以上数组的方式不好用模板语法，因此更改为对象形式

![](.\vue-img\vue基础\95.JPG)

##### **拿到全局模式下的state或其他模块的state**

getters方法内第二个参数是所有getter方法，第三个参数rootState是全局的state方法

```
getters: {
    sumWithRootCount (state, getters, rootState, rootGetters) {
      return state.count + rootState.count
    }
}
```

actions里方法第一个参数是ctx，这是个store对象，包含state，commit，rootState

```
const actions = {
  updateUserName ({ commit, state, rootState, dispatch }) {
    // rootState.appName 根vuex里的state
  },
  updateUserName ({ ctx, dispatch }) {
  }
}
```

如果你要调用其他模块或者全局里的mutations或action，需要加root:true，不然会报错（前提是声明了namespaced: true）

```
 dispatch('someOtherAction', null, { root: true }) 
```

##### 动态注册模块

模块下增加模块

```
//xx.vue组件中
registerModule () {
    this.$store.registerModule('todo', {
        state: {
        	//一个数组
            todoList: [
                '学习mutations',
                '学习actions'
            ]
        }
    })
	//给user模块添加一个todo模块
    this.$store.registerModule(['user', 'todo'], {
        state: {
        	//一个数组
            todoList: [
                '学习mutations',
                '学习actions'
            ]
        }
    })
},

computed: {
    ...mapState({
      todoList: state => state.user.todo ? state.user.todo.todoList : [],
    }),
}
```

**全局注册**

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

![](.\vue-img\vue基础\105.JPG)

#### 6.store.watch

第一个参数相当于getter

![](.\vue-img\vue基础\106.JPG)

当state变化时，会自动调用store.watch方法

#### 7.store.subscribe订阅

制作一些插件时，每次提交mutation后拿到回调函数

![](.\vue-img\vue基础\107.JPG)

type打印调用了哪个mutation

payload打印接收（传入）的参数

store.subscribeAction

![](.\vue-img\vue基础\109.JPG)

#### 8.plugin

vuex存到内存，想将它存到本地--------持久化存储，

```
//plugins.js
export default store => {
  //当本地有存储的state，转成对象，替换掉实例中的store-----replaceState
  if (localStorage.state) store.replaceState(JSON.parse(localStorage.state))
  store.subscribe((mutation, state) => {
    //对象转成字符串再存储
    localStorage.state = JSON.stringify(state)
  })
}

//index.js
import saveInLocal from './plugin/saveInLocal'
Vue.use(Vuex)
export default new Vuex.Store({
  strict: false,
  state,
  getters,
  mutations,
  actions,
  plugins: [ saveInLocal ]
})
```

#### 9.严格模式

```
export default new Vuex.Store({
  //如果是开发环境 严格模式开启，如果为生产环境 关掉，这样就不会报错
  strict: process.env.NODE_ENV === 'development',
  state,
  getters,
  mutations,
  actions,
  plugins: [ saveInLocal ]
})

//strict: true表示要在mutation里设置state的值
//process.env属性在Node中返回一个包含用户环境信息的对象，NODE_ENV是用户一个自定义的变量，在webpack中它的用途是判断生产环境或开发环境的依据的
```

#### 10.vuex双向绑定的问题

```
//state
const state = {
  appName: 'admin',
  stateValue: 'abc'
}

//mutation
const mutations = {
  SET_STATE_VALUE (state, value) {
    state.stateValue = value
  }
}

//组件中
<template>
	//方式一
  <input @input="handleStateValueChange" :value="value"/>
  
  	//方式二
  <input v-model="stateValue"/>
</template>

<script>
methods: {
    ...mapMutations([
          'SET_STATE_VALUE'
    ]),
    //方式一
    handleStateValueChange (val) {
        this.SET_STATE_VALUE(val)
    }
}

//方式二
computed: {
	stateValue: {
          get () {
            return this.$store.state.stateValue
          },
          set (val) {
            this.SET_STATE_VALUE(val)
          }
    },
}
</script>
```



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

**流程**

- updateComponent中实现了vdom的patch
- 页面首次渲染执行updateComponent
- data每次修改属性，set中执行updateComponent

#### 3.整体流程

![](.\img\14.JPG)

![](.\img\16.JPG)

![](.\img\17.JPG)

![](.\img\18.JPG)

### 十五.axios理解

解决跨域

代理

```
'/filesystem': {
    target: 'http://192.168.0.200:21771/FileSystem',
    ws: false,
    changeOrigin: true,
    pathRewrite: {
    	'^/filesystem': ''
    }
},
```

封装axios

```
//使用
import axios from './index'

export const getUserInfo = ({ userId }) => {
  return axios.request({
    url: '/index/getUserInfo',
    method: 'post',
    data: {
      userId
    }
  })
}


//index.js
import HttpRequest from '@/lib/axios'
const axios = new HttpRequest()
export default axios

//axios.js
import axios from 'axios'
import { baseURL } from '@/config'
import { getToken } from '@/lib/util'
class HttpRequest {
  constructor (baseUrl = baseURL) {
  	//基本路径
    this.baseUrl = baseUrl
    this.queue = {}
  }
  getInsideConfig () {
  	//全局的一些配置
    const config = {
      baseURL: this.baseUrl,
      headers: {
        //
      }
    }
    return config
  }
  distroy (url) {
    delete this.queue[url]
    if (!Object.keys(this.queue).length) {
      // Spin.hide()
    }
  }
  //拦截器方法
  interceptors (instance, url) {
    instance.interceptors.request.use(config => {
      // 添加全局的loading...
      if (!Object.keys(this.queue).length) {
        // Spin.show()
      }
      this.queue[url] = true
      config.headers['Authorization'] = getToken()
      return config
    }, error => {
      return Promise.reject(error)
    })
    instance.interceptors.response.use(res => {
      this.distroy(url)
      const { data } = res
      return data
    }, error => {
      this.distroy(url)
      return Promise.reject(error.response.data)
    })
  }
  request (options) {
    const instance = axios.create()
    options = Object.assign(this.getInsideConfig(), options)
    //添加一个拦截器
    this.interceptors(instance, options.url)
    return instance(options)
  }
}
export default HttpRequest
```

Mock.js模拟请求



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

#### git命令

密钥配对<https://blog.csdn.net/u013778905/article/details/83501204> 

1.克隆到本地 git clone  xxx

2.创建本地仓库 git init

git status 用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被Git tracked到。
git add . 把代码添加到Git的缓冲区
git commit -m 'xxx' 提交代码并编写日志

创建远程仓库（在github上）

先关联 git remote add origin < remote-project-repository-address > 

比如git remote add origin https://github.com/heqiliao007/xxx

再推送

git push origin master提交代码至线上

	//多人开发 
	git branch //查看分支
	git checkout -b xxx新创建一个分支
	git checkout xxx 切换到某个分支  
	
	git diff //查看不同
	git add .
	git commit -m "提交信息"//提交信息
	git push origin xxx分支 //提交到远程xxx分支
	
	git checkout master // 切换到master分支
	git pull origin master//先拉取一遍别人可能更新了
	
	git merge origin/xxx分支 // 把xxx分支上新增的内容合并到master分支上
	git push // 把master分支的内容提交到线上git push origin master
	
	//git 如何在A分支merge B分支的单个文件
	git checkout -p B file.txt
	
	git reset --soft HEAD^ //撤销本次提交，将本地仓库回滚到上一个版本，工作区和暂存区不变。
	HEAD^表示上个提交版本，HEAD^^表示上上个提交版本，HEAD~n代表前面第n个版本

gitbash 不能复制粘贴的使用快捷键 Ctrl/Shift+Insert 

### rollup打包

功能单一但极致，可集成可拓展

wangEditor集成用的rollup+gulp

### 权限控制

简单权限控制

iview-admin

 https://blog.csdn.net/qq_43436432/article/details/84374700 

 canTurnTo会通过第二个参数（用户的权限字段列表）进行匹配，如果当前页面，当前用户是有权限的，显示当前页面，否则跳转到401页面 
component，代表组件级别的路由权限  为true，代表可以访问这个页面
为false代表访问不了
这种方式有一个弊端，路由实例里每一个路由都要有一个name
并且不能和path重复

 进行权限过滤时候，过滤的就是routerMap数组，匹配不到直接显示404页面 

### **编辑配置editorconfig** 

最外层新建.editorconfig 搭配VScode插件**EditorConfig for VS Code**配置编辑器习惯，有了这个插件配置才会起作用

```
#开启
root = true
#所有文件都有效
[*]
charset = utf-8
#缩进
indent_style = space/tabs
#缩进尺寸
indent_size = 2
.....
```

